# Strider CI on AWS EC2
# Part 1: Strider Setup


## Prequisites

### Update host name
Edit `/etc/hostname` appending server hostname to the top 127.0.0.1 line to prevent from `sudo` from throwing errors.

You can check host name running `hostname` bash command.

**source:** [Error: sudo: unable to resolve host ip-xxx-xx-xx-xxx]( /tech/troubleshooting/ubuntu-unable-to-resolve-host-ip.md)

### Installing system packages

Script will install gcc compilers, node, update npm and install git

```
curl -L https://gist.githubusercontent.com/indieforger/20e345cf9dff308a7392/raw/setup-node-webserver.sh | bash
```

Install MongoDB
```
curl -L https://gist.githubusercontent.com/indieforger/20e345cf9dff308a7392/raw/install-mongodb.sh | bash
```

### Installing strider
```
npm install -g strider
```

### Starting mongodb
```
sudo service mongod start
```

## Strider user(s) setup

### Strider system user
```
sudo adduser --system --no-create-home strider
```
<!--
### Strider mongodb user

```
mongo --eval "db.createUser({user: 'strider', db: 'strider', roles: [{role: "dbOwner"}]})"
```
-->

### Strider application user

Run below command and follow instructions

```
strider addUser
```


## Port 80 redirection

### Instant iptables update

Redirect all trafic from port 80 to 3000 - default strider port.
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

<!--
SERVER_NAME="http://ci.indieforger.com" \
STRIDER_CLONE_DEST="/home/ubuntu/strider-builds/" \
PLUGIN_GITHUB_APP_ID="a79197d1586cfeaefd5c" \
PLUGIN_GITHUB_APP_SECRET="c83b937c527de075ef80eadb1cec442b65429eb8" \
pm2 start strider
-->

```bash
NODE_ENV="production" \
SERVER_NAME="http://ci.indieforger.com" \
STRIDER_CLONE_DEST="/home/ubuntu/strider-builds/" \
pm2 strider    
```

### Save configuration
```
pm2 save
```

### Configure pm2 start after system reboot

Run
```bash
pm2 startup ubuntu
```

It will output something like below, so just follow these instructions.
```
# [PM2] You have to run this command as root. Execute the following command:
#      sudo su -c "env PATH=$PATH:/usr/bin pm2 startup linux -u ubuntu --hp /home/ubuntu"
```
> source: https://futurestud.io/blog/pm2-restart-processes-after-system-reboot


```
sudo reboot
```

## sources
https://fosterelli.co/creating-a-private-ci-with-strider.html
https://futurestud.io/blog/strider-how-to-install-the-platform-and-plugins
