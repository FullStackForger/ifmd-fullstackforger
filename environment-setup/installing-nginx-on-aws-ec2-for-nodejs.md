# Installing NginX on AWS EC2 for NodeJS

This guide runs through nginx installation and configuration on Ubuntu Server 14.04 LTS AWS EC2 instance.

## Installing NginX

```
sudo apt-get install nginx -y
```

## Configuring NginX

NginX should already have sub-folders sites-available and sites-enabled at own location `/etc/nginx` so we need to create one for certificates and couple more for node upstreams and node endpoints.

```
cd /etc/nginx
#Folder for certificates
sudo mkdir ssl
#Folder for node upstreams and endpoints
sudo mkdir node-upstreams node-endpoints
```

## Nginx configuration

Create configuration backup **[Optional]**
```
sudo cp /etc/nginx/nginx.conf /etc/nginx/nginx.conf.default
```

To edit `nginx.conf` run:
```
vim /etc/nginx/nginx.conf
```

And paste below code.

**Note** `worker_process` should be equal to number of available CPU cores

```
user www-data www-data;
worker_processes  1;

events {
    worker_connections 768;
    # multi_accept on;
}

http {

    ##
    # Basic Settings
    ##

    sendfile on;
    tcp_nopush on;
    tcp_nodelay on;
    keepalive_timeout 65;
    types_hash_max_size 2048;

    # server_name_in_redirect off;

    include /etc/nginx/mime.types;
    default_type application/octet-stream;

    log_format compress '$remote_addr - $remote_user [$time_local]'
                       '"$request" $status $bytes_sent '
                       '"$http_referer" "$http_user_agent" "$gzip_ratio" '
                       '"$http_x_forwarded_for"';

    ##
    # Logging Settings
    ##

    access_log /var/log/nginx/access.log;
    error_log /var/log/nginx/error.log;

    ##
    # Gzip Settings
    ##

    gzip on;
    gzip_disable "msie6";

    # gzip_vary on;
    # gzip_proxied any;
    # gzip_comp_level 6;
    # gzip_buffers 16 8k;
    # gzip_http_version 1.1;
    # gzip_types text/plain text/css application/json application/x-javascript text/xml application/xml application/xml+rss text/javascript;

    ##
    # Virtual Host Configs
    ##

    include /etc/nginx/conf.d/*.conf;
    include /etc/nginx/sites-enabled/*;
}
```

Restart nginx and you are good to go

```
sudo service nginx restart
```

## Resources

### Tools
- [PM2](https://github.com/Unitech/pm2) - production process manager
- [Real time metrics for node](https://keymetrics.io/) for PM2

### Guides

- [goodbye-node-forever-hello-pm2](http://devo.ps/blog/goodbye-node-forever-hello-pm2/)
- [How To Use PM2 to Setup a Node.js Production Environment On An Ubuntu VPS](https://www.digitalocean.com/community/tutorials/how-to-use-pm2-to-setup-a-node-js-production-environment-on-an-ubuntu-vps)
- [NodeJS in production](http://blog.carbonfive.com/2014/06/02/node-js-in-production/)
- [How To Set Up a Node.js Application for Production on Ubuntu 14.04](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-node-js-application-for-production-on-ubuntu-14-04)
- [Understanding Nginx HTTP Proxying, Load Balancing, Buffering, and Caching](https://www.digitalocean.com/community/tutorials/understanding-nginx-http-proxying-load-balancing-buffering-and-caching)
- [Nginx SSL Certificate Installation](https://www.digicert.com/ssl-certificate-installation-nginx.htm)
- [Basic nginx configuration](https://www.linode.com/docs/websites/nginx/basic-nginx-configuration)
- [NginX Optimisation: understanding sendfile, tcp_nodelay tcp_nopush](https://t37.net/nginx-optimization-understanding-sendfile-tcp_nodelay-and-tcp_nopush.html)
- [NginX Performance tunning](http://dak1n1.com/blog/12-nginx-performance-tuning)