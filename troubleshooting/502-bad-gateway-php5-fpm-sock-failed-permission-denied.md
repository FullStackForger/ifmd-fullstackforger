# 502 Bad Gateway caused by Permission Denied

While setting up indieforger.com ubuntu server i run into **502 Bad Gateway** caused by Permission Denied and logged as:

```
connect() to unix:/var/run/php5-fpm.sock failed (13: Permission denied) while connecting to upstream, client: XX.XX.XX.XX, server: indieforger.com, request: "GET / HTTP/1.1", upstream: "fastcgi://unix:/var/run/php5-fpm.sock:", host: "indieforger.com"
```

## Qucik fix

Fast fix is as suggested by [Chris](http://chriskief.com/2014/05/07/nginx-php5-fpm-and-permission-denied-errors/) is to edit: */etc/php5/fpm/pool.d/www.conf*
php-fpm config file setting, uncommenting `listen.mode` and setting it to:
```
listen.mode = 0666
```

## Good fix

While first solution will work ok I am not entirely sure about how secure it is. I think it is better to leave `listen.mode` set to it's default values `0660`and make sure nginx worker user is the same as php-fpm user.

running `ps -aux |grep nginx` shows that worker is run by *nobody* user.
```
root    22965  0.0  0.0  86008  1420 ?        Ss   15:30   0:00 nginx: master process /usr/sbin/nginx
nobody  22966  0.0  0.0  86584  2600 ?        S    15:30   0:00 nginx: worker process
```

Executing `-aux |grep php-fpm` indicates that *www-data* is owner of php-fpm workers.
```
root     22958  0.0  0.4 375996 18848 ?        Ss   15:30   0:00 php-fpm: master process (/etc/php5/fpm/php-fpm.conf)
www-data 22961  0.0  0.1 375996  6380 ?        S    15:30   0:00 php-fpm: pool www
```

Fix to the problem then should be setting *www-user* in `/etc/nginx/nginx.conf`
```
sudo vim /etc/nginx/nginx.conf
```

And add below at the very top of the file
```
user www-data www-data;
worker_processes  1;
```

Restart services
```
sudo service php5-fpm restart && sudo service nginx restart
```

Confirm ports with `sudo lsof -nP -i | grep LISTEN`:
```
sshd      1020     root    3u  IPv4   8959      0t0  TCP *:22 (LISTEN)
sshd      1020     root    4u  IPv6   8961      0t0  TCP *:22 (LISTEN)
mongod    1162  mongodb    6u  IPv4   9415      0t0  TCP 127.0.0.1:27017 (LISTEN)
node      2738   ubuntu   16u  IPv4  12084      0t0  TCP 127.0.0.1:8001 (LISTEN)
mysqld   17659    mysql   10u  IPv4 147666      0t0  TCP 127.0.0.1:3306 (LISTEN)
nginx    22965     root    6u  IPv4 165063      0t0  TCP *:80 (LISTEN)
nginx    22966 www-data    6u  IPv4 165063      0t0  TCP *:80 (LISTEN)
```

And check your site. Voila!

Sources:
 - http://nginx.org/en/docs/ngx_core_module.html#user
 - http://php.net/manual/en/install.unix.nginx.php
 - http://chriskief.com/2014/05/07/nginx-php5-fpm-and-permission-denied-errors/

