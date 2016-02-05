#Installing NginX on OSX with MacPorts for NodeJS

## Mack Ports

If you don't have MackPorts installed download and install pkg file from [mackports.org](https://www.macports.org/install.php)

From the console run `sudo port selfupdate` to update Mack Ports

## NginX installation with MacPorts

### NginX installation

To list full range of all available extensions/options execute:

```
sudo port variants nginx 
```

**Note:** this instruction assumes you want to use NginX with NodeJS.

To install with ssl extension and redis [cashing](http://wiki.nginx.org/HttpRedis) enabled run:

```
sudo port install nginx +ssl +redis
```

### NginX setup

```
cd /opt/local/etc/nginx
#Folders for Virtual Host configs
sudo mkdir conf.d 
sudo mkdir sites-available sites-enabled 
#Folder for certificates
sudo mkdir ssl
#Folder for node upstreams and endpoints
sudo mkdir node-upstreams node-endpoints
```

## Nginx configuration

Create configuration backup **[Optional]**
```
sudo cp nginx.conf nginx.conf.default
```

To edit `nginx.conf` run:
```
vim /opt/local/etc/nginx/nginx.conf
```

Once opened replace its content with below configuration:

```
worker_processes  2;

error_log  /opt/local/var/log/nginx/error.log;
#error_log  /opt/local/var/log/nginx/error.log  notice;
#error_log  /opt/local/var/log/nginx/error.log  info;

events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;
    client_max_body_size 100m;

    log_format compress '$remote_addr - $remote_user [$time_local]'
                       '"$request" $status $bytes_sent '
                       '"$http_referer" "$http_user_agent" "$gzip_ratio" '
                       '"$http_x_forwarded_for"';

    # access_log /var/log/nginx/access.log main buffer=32k;

    # Virtual Host Configs
    include conf.d/*.conf;
    include sites-enabled/*;
}
```
Save and exit with `[esc]` and `:wq` command.

### NginX restarting

Stop Nginx with:
```
sudo port unload nginx
```

Start Nginx with: 
```
sudo port load nginx
```

### NginX auto start upon machine start

If you want nginx auto start after machine reboot run:
```
sudo launchctl load -w /Library/LaunchDaemons/org.macports.nginx.plist
```

## Simple NodeJS application for NginX test

Create app folder and cd into it:
```
cd
mkdir Workspace/tmp/simple-node-app -p
cd Workspace/tmp/simple-node-app
```

Create `package.json` file.
```
vim package.json
```

Now paste in below `package.json` file content
```
{
  "name": "simple-node-app",
  "version": "1.0.0",
  "description": "Simple HapiJS app",
  "main": "index.js",
  "dependencies": {
    "hapi": "^8.1.0"
  }
}
```

Install node dependency packages with:
```
npm install
```

Create `index.js` file with:
```
vim index.js
```

Paste below `index.js` content into open file.
```
var Hapi = require('hapi'),
    server = new Hapi.Server();

server.connection({
    host: 'localhost',
    port: 3000
});

server.route({
    path: "/", 
    method: "GET", 
    handler: function (response, reply) {
        reply("Welcome!")
    }
});

server.start();
```

Start server with
```
node index.js
```

Navigate to `http://localhost:3000` and you should see 'Welcome!'.


## Configuring nginx domain name for node application

### NginX configig for simple node app available from dev.localhost

Create `dev.localhost` file for your node applications:
```
sudo vim /opt/local/etc/nginx/sites-available/dev.localhost
```

Paste below configuration into the open file
```
# Reverse proxy definitions to services
include node-upstreams/dev.localhost.conf;

server {
    listen       80;
    server_name  dev.localhost;
    client_max_body_size 1000M;

    # Endpoint mappings
    include node-endpoints/dev.localhost.conf;

    error_page  404              /404.html;
    error_page  500 502 503 504  /50x.html;
}
```

### Create NginX upstream

Create Simple Node App upstream
```
sudo vim /opt/local/etc/nginx/node-upstreams/dev.localhost.conf
```

Paste below configuration into it
```
upstream simple-node-app {
   #least_conn;
   server 127.0.0.1:3000;
}
```

### Create End-Point

Create Simple Node app end-point
```
sudo vim /opt/local/etc/nginx/node-endpoints/dev.localhost.conf
```

Paste below configuration into it
```
location / {
    rewrite ^/(.*) /$1 break;
    proxy_set_header X-Real-IP  $remote_addr;
    proxy_set_header X-Forwarded-For $remote_addr;
    proxy_set_header Host $host;
    proxy_pass http://simple-node-app/;
}
```

### Update `hosts` file 

Open `hosts` file
```
sudo vim /private/etc/hosts
```

And add mapping hostname to local ip address:
```
127.0.0.1   dev.localhost
```

### Enable http://dev.localhost
```
cd /opt/local/etc/nginx/sites-enabled/
sudo ln -s ../sites-available/dev.localhost dev.localhost
```

### Reload NginX
```
sudo port unload nginx && sudo port load nginx
```
If everything is set up properly you should see 'Welcome!' when
you open http://dev.localhost in your browser.


### Tweaking `.bash_profile` for handy aliases

```
# nginx start, stop, restart aliases
alias nginx-start="sudo launchctl load -w /Library/LaunchDaemons/org.macports.nginx.plist"
alias nginx-stop="sudo launchctl unload -w /Library/LaunchDaemons/org.macports.nginx.plist"
alias nginx-restart="nginx-stop; nginx-start;"
```

## Sources:
- [launchctl interface](http://ss64.com/osx/launchctl.html)
- [NginX server_names](http://nginx.org/en/docs/http/server_names.html)
- [Nginx + PHP with MacPorts](https://gist.github.com/renjunkui/1267057)
- [A Guide to PHP, MySQL and Nginx on Macports](http://aaronbonner.io/post/44973182283/a-guide-to-php-mysql-and-nginx-on-macports)
