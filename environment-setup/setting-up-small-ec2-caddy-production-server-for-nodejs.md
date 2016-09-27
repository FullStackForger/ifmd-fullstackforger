# Setting up small ec2 Caddy production server for NodeJS

## Requirements

- [x] posix (ubuntu) based
- [x] SSL enabled for https
- [x] autostart after reboot
- [x] easy setup
- [ ] system log files

## Server setup

Our production server is going to be Ubuntu 14.04.3 LTS 64-bit installed on AWS EC2 instance.

We will need git, gcc compilers, node and updated npm. You can install all of it by running below command or copy [script content](https://gist.githubusercontent.com/indieforger/20e345cf9dff308a7392/raw/setup-node-webserver.sh) and paste it into the terminal.

```
curl -L https://gist.githubusercontent.com/indieforger/20e345cf9dff308a7392/raw/setup-node-webserver.sh | bash
```

> Ubuntu is an arbitrary choice.  If you using different system make sure you have all mentioned packages installed before proceeding.

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
sudo wget https://github.com/mholt/caddy/releases/download/v0.8.3/caddy_linux_amd64.tar.gz
sudo tar -xvf caddy_linux_amd64.tar.gz
sudo chown -R caddy ../bin
sudo chgrp -R caddy ../bin
```

You can get latest release version from [caddy releases](https://github.com/mholt/caddy/releases) page.

## Binding to port 80

Set permissions to bind to ports `80` and `443` as described in **Binding to ports 80 and 443** section in the official caddy [docs](https://caddyserver.com/docs/automatic-https)
```
sudo setcap cap_net_bind_service=+ep /home/caddy/bin/caddy
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
echo 'localhost:80' >  ~/Caddyfile
# run server
~/bin/caddy -conf="/home/caddy/Caddyfile" -agree
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

script
    cd /home/caddy/
    bin/caddy -conf="/home/caddy/Caddyfile" -agree
end script

```

**Note:** To make it work you also have to edit Cadyfile, specifying setting `root` directive to folder root, eg:

```
localhost:80 test.yourdomain.com:80 {
  root  /home/caddy/www
}
```

## Known issues

### Registration rate limiting error

Found in `/var/log/upstart/caddy.log`

>```
>Error creating new registration :: Too many registrations from this IP
>```

From [community.letsencrypt.org](https://community.letsencrypt.org/t/unexpected-registration-rate-limiting-error/2157/4):

> We're looking into this; it appears to mostly affect IPv6 clients. We're going to adjust some limits -- and drop from 1 week to 1 day windows -- on our side and I'll post back.

As discussed on caddy [github discussion](https://github.com/mholt/caddy/issues/383#issuecomment-162163145) solution is to temporarily disable IPv6.

```
sudo sysctl -w net.ipv6.conf.all.disable_ipv6=1
```

Issue your Let's Encrypt certificate, and re-enable IPv6 with:

```
sudo sysctl -w net.ipv6.conf.all.disable_ipv6=0
```

### Certificate expired

For some reason Caddy server doesn't always auto-refresh certificates.
The best way to deal with the problem is to manually revoke certificate.


1. Stop caddy service
```
sudo service caddy stop
```
2. Tail logs for prseview of the progress
```
sudo tail /var/log/upstart/caddy.log -f
```
3. Revoke certificates
```
caddy -revoke="expired.cert.domain.com"
```
4. Start caddy server
```
sudo service start
``` 


## Useful tools

- [Symantec CryptoReport](https://cryptoreport.websecurity.symantec.com/checker/views/certCheck.jsp)
- [Certificate Search](https://crt.sh)

## Sources:

Here are links to resources used in the guide.

- https://denbeke.be/blog/servers/running-caddy-server-as-a-service/
- https://github.com/mholt/caddy/issues/388
- https://github.com/jhillyerd/inbucket/blob/master/etc/ubuntu-12/inbucket-upstart.conf
- https://bash.cyberciti.biz/guide/Sending_signal_to_Processes
