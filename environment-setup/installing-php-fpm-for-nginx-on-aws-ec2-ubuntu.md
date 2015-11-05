#Installing PHP-FPM for NginX on AWS EC2 Ubuntu instance

This post shows how to auto install the latest version of PHP5 with few bash commands.

## Automated Installation

### Add this package-repository first.

`ondrej/php5` repository  provides the latest PHP version of, usually in a few days after it was been officially released.

```
sudo add-apt-repository ppa:ondrej/php5
```

> **Note**  
> If you  getting an error you will need `python-software-properties`
> ```
> sudo apt-get update
> sudo apt-get install python-software-properties
> ```

### Update & Upgrade
```
sudo apt-get update -y
sudo apt-get upgrade -y
```

### Install PHP

```
sudo apt-get install php5 php5-cgi php5-fpm -y
sudo apt-get install  php5-curl php5-gd php5-imagick php5-mcrypt php5-memcache php5-mysql php5-xdebug -y
```

Check the installed version of PHP via
```
php5 -v
```

## Configuration

### PHP-FPM Configs

Edit config
```
sudo vim /etc/php5/fpm/pool.d/www.conf
```

And make sure all the following options are uncommented and set to as follows:
```
; Unix user/group of processes
user = www-data
group = www-data

; The address on which to accept FastCGI requests.
listen = /var/run/php5-fpm.sock

; Set permissions for unix socket ...
listen.owner = www-data
listen.group = www-data
listen.mode = 0660
```

### Global NginX configuration

```
sudo vim /etc/nginx/nginx.conf
```

At the very top of the file you should have:
```
user www-data www-data;
worker_processes  1;
```

### NginX site configuration
 
```
sudo vim /etc/nginx/sites-enabled/yoursite.com
```

```
server {
  listen 80;
  server_name   yoursite.com;
  root      /var/www/php-test;
  index     index.php index.html index.htm;

  location ~ \.php$ {
    include fastcgi_params;
    fastcgi_pass unix:/var/run/php5-fpm.sock;    
    fastcgi_param SCRIPT_FILENAME $document_root/$fastcgi_script_name;
  }

  location / { }
}
```

## Sources:

 - http://php.net/manual/en/install.unix.nginx.php
 - https://www.digitalocean.com/community/tutorials/understanding-nginx-http-proxying-load-balancing-buffering-and-caching
 - http://stackoverflow.com/questions/23844761/upstream-sent-too-big-header-while-reading-response-header-from-upstream
 - https://gist.github.com/magnetikonline/11312172#determine-fastcgi-response-sizes

 
