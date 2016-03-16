# Strider CI on AWS EC2 <br />Part 1: Strider Setup

## Prerequisites

### Server

This guide will show you how to run Strider Continuous Deployment server on AWS EC2 Ubuntu instance.
Before you proceed, and if you haven't done it yet, go ahead and create yourself an Ubuntu 14.04 LTS 64bit server.
Configuring DNS to point to `ci.your-server-name.com` domain is not necessary but doing it will make your life easier. If you are with AWS go ahead and use Route53 service to do so before proceeding.  

SSH to your server using .pem keys. If you need help with that you can take a look at
[ssh configuration with .pem keys](../../aws/ec2-ssh-configuration-with-pem-keys).

### Host name update
Edit `/etc/hostname` appending server hostname to the top 127.0.0.1 line to prevent from `sudo` from throwing errors.

You can check host name running `hostname` bash command.

**source:** [Error: sudo: unable to resolve host ip-xxx-xx-xx-xxx]( /tech/troubleshooting/ubuntu-unable-to-resolve-host-ip)

### System packages

Run below script to install gcc compilers, node, update npm and install git

```
curl -L https://gist.githubusercontent.com/indieforger/20e345cf9dff308a7392/raw/setup-node-webserver.sh | bash
```

Install MongoDB as well
```
curl -L https://gist.githubusercontent.com/indieforger/20e345cf9dff308a7392/raw/install-mongodb.sh | bash
```

### Installing strider

Strider can be installed with npm so go ahead and do so.

```
npm install -g strider
```

### Starting mongodb

```
sudo service mongod start
```

## Strider user(s)

We are going to create separate deployment user called `strider` as both system and application user. It is always good idea to do so. Deployment user should have minimal required privileges to reduce the risk of compromising the system.

### System user

Sole responsibilty of system deployment user is running CI server. It doesn't need to have neither home directory or tty. Lets go head and create system user called `strider`.

```
sudo adduser --system --no-create-home strider
```


### Application user

We also need application user, run below command and follow instructions.
Put details you will want to use to log into strider.

```
strider addUser
```

### Strider mongodb user

We are going to skip this step for now for simplicity. However, if CI server is exposed publicly, securing mongodb is always a good idea.

<!--
todo: complete installation with mongo user
```
mongo --eval "db.createUser({user: 'strider', db: 'strider', roles: [{role: "dbOwner"}]})"
```
-->

## Strider builds directory
Strider will need a directory to store all project build files. Let's call it `strider-builds` and put it in our home directory.

```
mkdir -p /home/ubuntu/strider-builds
```

## Port 80 redirection
<!-- todo: replace redirection to proxying with caddy -->

### Instant iptables update

Redirect all traffic from port 80 to 3000 - default strider port.
```
sudo iptables -t nat -A PREROUTING -i eth0 -p tcp --dport 80 -j REDIRECT --to-port 3000
```

### iptables update after system reboot

Edit `/etc/rc.local` file and add below line to setup redirection after instance boots up.
**Note:** Same as above but without sudo.

```
iptables -t nat -A PREROUTING -i eth0 -p tcp --dport 80 -j REDIRECT --to-port 3000
```

> Source: http://stackoverflow.com/a/16573737


## pm2

PM2 is great process manager with awesome logging capabilities.

### Installing PM2

```
npm install -g pm2
```

### Start strider with pm2

Now, you should start strider with some environment properties.

```bash
NODE_ENV="production" \
SERVER_NAME="http://ci.your-server-name.com" \
STRIDER_CLONE_DEST="/home/ubuntu/strider-builds/" \
pm2 start strider
```

Save PM2 configuration.
```
pm2 save
```

You can now navigate to `http://ci.your-server-name.com` and log into Strider.


### PM2 auto start after reboot

PM2 comes with `startup` command that will help you to auto start pm2 start after system reboot.

```bash
pm2 startup ubuntu
```

It should output something like below, so just follow these instructions.
```
# [PM2] You have to run this command as root. Execute the following command:
#      sudo su -c "env PATH=$PATH:/usr/bin pm2 startup linux -u ubuntu --hp /home/ubuntu"
```
> source: https://futurestud.io/blog/pm2-restart-processes-after-system-reboot

Now you should be able to restart the server.
```
sudo reboot
```

Wait a minute or two and refresh your Strider in your browser.

## sources
https://fosterelli.co/creating-a-private-ci-with-strider.html
https://futurestud.io/blog/strider-how-to-install-the-platform-and-plugins
