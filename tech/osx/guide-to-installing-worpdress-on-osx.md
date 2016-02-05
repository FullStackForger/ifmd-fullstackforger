# Guide to installing Wordpress on Mac

## Preparation

### Install MySQL, NginX with PHP-FPM
<!-- todo: links to other guides -->

### Download latest wordpress 
<!-- todo: links to setting up ssh -->
```
wget http://wordpress.org/latest.tar.gz
```
Then unzip the package using: 
```
tar -xzvf latest.tar.gz 
```

WordPress package will be extractec into a folder called wordpress inside of tar.gz package parent directory. 


### Configure nginx

__NOTE:__ Update `blog.name.dev.conf` with appropriate dev domain.

Update nginx /opt/local/etc/nginx/sites-available/blog.name.dev.conf
```
server {
  listen 80;
  server_name  blog.name.dev;

  root /Users/Marek/Workspace/Innocentio/wp.innocentio.com;
  index               index.php index.html index.htm;

  location ~ \.php$ {
    include fastcgi_params;
    fastcgi_pass  127.0.0.1:9000;
    fastcgi_param SCRIPT_FILENAME $document_root/$fastcgi_script_name;
  }

  error_page  404              /404.html;
  error_page  500 502 503 504  /50x.html;
}

```
Remember to symlink 
```
cd /opt/local/etc/nginx/sites-enabled/
sudo ln -s ../sites-available/blog.name.dev.conf ./blog.name.dev.conf
```

Update `/etc/hosts`
```
127.0.0.1   blog.name.dev
```
#### Sources:

 - http://codex.wordpress.org/Nginx#General_WordPress_rules
 
### Create MySQL database and user for wordpress

Launch mysql CLI
```
mysql -u root -p
```
```
create database wordpress;
create user wordpress;
grant all on wordpress.* to 'wordpress'@'localhost' identified by 'wp_password';
flush privileges;
```


### Post-install configuration

#### Install updates and plugins without ftp

Add bellow lines to `wp-config.php`:
```
/* enables updates and plugin installation without ftp */
define('FS_METHOD', 'direct');
```

Cd to your wordpress directory and set write privileges for `wp-content` and `wp-content/plugins`:

```
chmod 777 wp-content/
chmod 777 wp-content/plugins
chmod 777 wp-content/themes
```

**Note: ** It is safer to reverse privileges back to original after you have finished plugin installation.




