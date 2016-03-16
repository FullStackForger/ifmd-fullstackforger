# Installing PHP FPM for NginX with MacPorts on OSX

## Install PHP with fastCGI

```
sudo port install php55 php55-cgi php55-fpm
sudo port install  php55-curl php55-gd php55-http php55-iconv php55-imagick php55-mcrypt php55-memcache php55-mysql php55-xdebug php55-openssl php5-zlib
``` 
note: 
we are installing _php55-memcache_ not _php55-memcached_ beacuse Wordpress W3-cache module doesn't support latter.

### Create config files
```
cd /opt/local/etc/php55
sudo cp php.ini-development php.ini
sudo cp php-fpm.conf.default php-fpm.conf
```

### Select php55 as default
```
sudo port select php php55
```

### Loading and unloading php-fpm
```
sudo launchctl load -w /Library/LaunchDaemons/org.macports.php55-fpm.plist
```

```
sudo launchctl unload /Library/LaunchDaemons/org.macports.php55-fpm.plist
```
### Test PHP FPM

Run any of below commands to make sure PHP-FPM is listening on port 9000:
```
sudo lsof -Pni4 | grep LISTEN | grep php
sudo lsof -nP -iTCP:9000 -sTCP:LISTEN
```
Using memchased with w3cache info:  
https://rtcamp.com/tutorials/php/memcache/

Installing memcached with mac-ports:   
https://glassonionblog.wordpress.com/2012/02/18/installing-memcached-on-mac-os-x-snow-leopard/
http://archive.robwilkerson.org/2010/06/07/install-technicalmemcached-on-osx-via-macports/
http://stackoverflow.com/questions/24454494/macports-memcached-restart-mac-os-x


## Configuring NginX with default settings (with TCP/IP on port 9000)

By default PHP-FPM listens on port 9000 for TCP/IP connections.

### Example configuration

Update __server_name__ and __root__ directives.

```
server {
  listen 80;
  server_name examplesite.com;
  root /path/to/site/directory/examplesite.com;
  index               index.php index.html index.htm;

  location ~ \.php$ {
    include fastcgi_params;
    fastcgi_pass  127.0.0.1:9000;
    fastcgi_index index.php;
    fastcgi_param SCRIPT_FILENAME $document_root/$fastcgi_script_name;
  }

  location / {
      autoindex on;
  }
}
```

Update `/etc/hosts`
```
127.0.0.1 examplesite.com
```

### Reload NginX
```
sudo port unload nginx && sudo port load nginx
```

## Configuring NginX to run via socket

### `php-fpm.conf`
Open config file with: `sudo vim /opt/local/etc/php55/php-fpm.conf`  
Uncomment following directives setting them to:
```
; Unix user/group of processes
user = www
group = www

; The address on which to accept FastCGI requests
listen = /tmp/php-fpm.sock

; Set permissions for unix socket
listen.owner = www
listen.group = www
listen.mode = 0666
```

### `nginx.conf`
Open config file with: `sudo vim /opt/local/etc/nginx/nginx.conf`
and add below line at the very top of the config.

```
user=www
```

### `opt/local/etc/nginx/sites-available/your-site.config`
Open your site nginx configuration file and replace line `fastcgi_pass  127.0.0.1:9000` with location to php-fpm socket, as shown below.

```
  location ~ \.php$ {
    include fastcgi_params;    
    # fastcgi_pass  127.0.0.1:9000;
    fastcgi_pass unix:/tmp/php-fpm.sock;
    fastcgi_param SCRIPT_FILENAME $document_root/$fastcgi_script_name;
  }
```

### Reload NginX and PHP-FPM services
```
sudo port unload nginx && sudo port load nginx
sudo launchctl unload /Library/LaunchDaemons/org.macports.php55-fpm.plist
sudo launchctl load -w /Library/LaunchDaemons/org.macports.php55-fpm.plist
```

Now running below command should NOT return anything.
```
sudo lsof -nP -iTCP:9000 -sTCP:LISTEN
```

## Add yourself to www group

```
sudo dseditgroup -o edit -a `whoami` -t user _www
sudo dseditgroup -o edit -a www -t user staff
```
## Tweaking `.bash_profile` for handy aliases

### NginX

It was already shown in previous post [todo: link] but in case you haven't seen it here it goes again.

```
# nginx start, stop, restart aliases
alias nginx-start="sudo launchctl load -w /Library/LaunchDaemons/org.macports.nginx.plist"
alias nginx-stop="sudo launchctl unload -w /Library/LaunchDaemons/org.macports.nginx.plist"
alias nginx-restart="nginx-stop; nginx-start;"
```

### PHP-FPM

```
#php-fpm start, stop, restart aliases
alias php-fpm-start="sudo launchctl load -w /Library/LaunchDaemons/org.macports.php55-fpm.plist"
alias php-fpm-stop="sudo launchctl unload /Library/LaunchDaemons/org.macports.php55-fpm.plist"
alias php-fpm-restart="php-fpm-stop; php-fpm-start;"
```
