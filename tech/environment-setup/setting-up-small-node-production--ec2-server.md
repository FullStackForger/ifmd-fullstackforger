# Setting up small NodeJS production ec2 server

## Requirements

- [x] posix (ubuntu) based
- [x] SSL enabled for https
- [x] autostart after reboot
- [x] easy setup
- [ ] system log files

## Server setup

Our production server is going to be Ubuntu 14.04.3 LTS 64-bit installed on AWS EC2 instance.

We will need git, gcc compilers, node and updated npm. You can install all of it by running below command.

```
curl -L https://gist.githubusercontent.com/indieforger/20e345cf9dff308a7392/raw/setup-node-webserver.sh | bash
```

> Ubuntu is an arbitrary choice so if you using different system make sure you have mention packages installed before proceeding.

## Caddy user

Again, quite arbitrary but it might be good idea to have a system user with access to run web-server. In example we will create system user `caddy` used to start, stop our web-server.

```
# add user without password and not interactive
sudo adduser --disabled-password --gecos "" caddy
```

## Installing Caddy web server

We are downloading and unpacking caddy into `caddy` user folder.
```
sudo mkdir -p /home/caddy/bin
cd /home/caddy/bin
sudo wget https://github.com/mholt/caddy/releases/download/v0.8.1/caddy_linux_amd64.tar.gz
sudo tar -xvf caddy_linux_amd64.tar.gz
sudo chown -R caddy ../bin
sudo chgrp -R caddy ../bin
```

> Get latest release version from [caddy releases](https://github.com/mholt/caddy/releases) page

Set permissions to bind to ports `80` and `443` as described in **Binding to ports 80 and 443** section in the official caddy [docs](https://caddyserver.com/docs/automatic-https)
```
sudo setcap cap_net_bind_service=+ep caddy
```

## Testing Caddy with Caddyfile config

Lets log in as `caddy` user

```
# log in as caddy
sudo su caddy
cd
# create test directory
mkdir -p ~/www/
```

Caddy comes with number of directives that enable various behaviors. Check [caddyserver.com/docs](https://caddyserver.com/docs) for more details.

For now lets try super simple configuration with example `index.html` file.

```
# create test index file
echo 'it is working' >  ~/www/index.html
# configure caddy
echo 'localhost:80' >  ~/www/Caddyfile
# run server
~/bin/caddy -conf="/home/caddy/www/Caddyfile"
```

From another terminal run `curl localhost`. You should see:
```
it is working
```

## Auto starting after reboot

We will use upstart config file saved into `/etc/init/caddy.conf`.
Create the file as `sudo` user and paste into it bellow content.

```
description "Caddy Server startup script"

start on runlevel [2345]
stop on runlevel [016]

setuid caddy
setgid caddy

respawn
respawn limit 10 5

limit nofile 4096 4096

setcap 'cap_net_bind_service=+ep' $program

pre-start script    
    setcap 'cap_net_bind_service=+ep' /home/caddy/bin/caddy
end script

script
    /home/caddy/bin/caddy -conf="/home/caddy/www/Caddyfile"    
end script
```

### NPM

Execute bellow commands to setup npm
```
mkdir ~/.npm-global
npm config set prefix '~/.npm-global'
printf "\nexport PATH=~/.npm-global/bin:$PATH\n" >> ~/.profile && source ~/.profile
```
