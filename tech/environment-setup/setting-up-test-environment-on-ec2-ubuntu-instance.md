# Setting up production environment on EC2 Ubuntu instance

## Prequisites

```
sudo apt-get update 
sudo apt-get upgrade -y
```
### Libraries

```
sudo apt-get install build-essential libssl-dev -y
```

### Node

> The easiest and fastest way is [Installing node via package manager](https://github.com/joyent/node/wiki/installing-node.js-via-package-manager)

But in practice, to avoid some drawbacks with `sudo npm install` and benefit from easy switching between node versions installing nodel via [NVM](https://github.com/creationix/nvm) seens nuch better.
```
curl https://raw.githubusercontent.com/creationix/nvm/v0.23.3/install.sh | bash
source ~/.bashrc
```

Then install stable node version, which is 0.12.0 at the time of writing.
Also set it as default to be used with new shell and after server reboot.
```
nvm install stable
nvm alias default stable
```


### MongoDB

> [Installing MongoDB on Ubuntu](http://docs.mongodb.org/manual/tutorial/install-mongodb-on-ubuntu/)

```
# Import the public key used by the package management system
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10

# Create a list file for MongoDB
echo "deb http://repo.mongodb.org/apt/ubuntu "$(lsb_release -sc)"/mongodb-org/3.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.0.list

# Reload local package database
sudo apt-get update

# Install MongoDB
sudo apt-get install -y mongodb-org
```

Now you can manage mongod service with following commands

```
sudo service mongod start
sudo service mongod stop
sudo service mongod status
```

### NginX

```
sudo apt-get install nginx -y
```

Now you can manage nginx service with following commands

```
sudo service nginx start
sudo service nginx stop
sudo service nginx status
```

NginX should boot up automaticaly, you verify that with `sudo service nginx status` but also after you navigate to public dns or public ip of your ec2 instance you should see:

> #### Welcome to nginx!
> 
> If you see this page, the nginx web server is successfully installed 
> and working. Further configuration is required.
> 
> For online documentation and support please refer to nginx.org.  
> Commercial support is available at nginx.com.
> 
> Thank you for using nginx.

Make sure NginX starts after server reboot

```
sudo update-rc.d nginx defaults
```
### Git

Copy and paste below installation instruction
```
sudo add-apt-repository ppa:git-core/ppa -y
sudo apt-get update
sudo apt-get install git -y
```

Check version with
```
git --version
```


## Configuration
 
### Route53

Add two A type record sets with values as in example shown below where `x.x.x.x` stands for your instance IP address or better **elastic IP address** associated with the instance.

- first with blank name:

```
name: yourdomain.com
type: A 
value: x.x.x.x
TTL: 300
```

 - second with 'www' as name:

```
name: www.yourdomain.com
type: A 
value: x.x.x.x
TTL: 300
```

### NginX

<!-- todo: link missing -->
Check out "Installing NginX on AWS EC2 for NodeJS" for configuration and examples.

To get nginx configuration wget nginx.conf gist.

```
sudo wget -O /etc/nginx/nginx.conf https://gist.githubusercontent.com/innocentio/ade7a21aacfcefce8fbd/raw
```

Alternatively manualy review, edit `/etc/nginx/nginx.conf` and paste [NginX configuration for NodeJS web app on AWS EC2](https://gist.github.com/innocentio/ade7a21aacfcefce8fbd)

#### Log rotation with log rotate

To avoid heavy logs edit `sudo nano /etc/logrotate.d/nginx`.
 - Change 'weekly' to 'daily' to rotate logs daily
 - Update 'rotate' for number of archives 

### MongoDB

Following official guide: [MongoDB on Linux](http://docs.mongodb.org/manual/administration/production-notes/#mongodb-on-linux)

#### Disable transparent huge pages
Jira [ticket](https://jira.mongodb.org/browse/DOCS-2131)

Update `/etc/rc.local` to disable `transparen_hugepages`

```shell
sudo wget -O /etc/rc.local https://gist.githubusercontent.com/innocentio/2bc64ba3c7e98daf955e/raw
```

Or alternatively copy and paste below code manualy.

```shell
mongo_restart=0
if test -f /sys/kernel/mm/transparent_hugepage/enabled; then
   mongo_restart=1
   echo never > /sys/kernel/mm/transparent_hugepage/enabled
fi
if test -f /sys/kernel/mm/transparent_hugepage/defrag; then
   mongo_restart=1
   echo never > /sys/kernel/mm/transparent_hugepage/defrag
fi
if [ "$mongo_restart" = 1 ]; then
   service mongod restart
fi
```

#### Monitoring

Monitoring alternatives:
 - [MMS](https://mms.mongodb.com/) - Mongo Monitoring System
 - Munin - requires [munin plugin](https://mms.mongodb.com/)
 

### Node

#### Packages

<!--
    todo: doesn't want to install without sudo and if I do it messes up ~/.npm/node_moduels privileges
```
sudo npm install -g pm2
# or
npm install pm2 -g --unsafe-perm
```
-->

#### Logging

 - [Scribe.js](https://github.com/bluejamesbond/Scribe.js)
 - [winston](https://github.com/winstonjs/winston)
 - [npmlog](https://github.com/npm/npmlog)
 - [nodejs-log-server](https://github.com/afshinm/nodejs-log-server) opensource (dependencies: mongoose, express)
 - [log4js-node](https://github.com/nomiddlename/log4js-node)

#### Monitoring

 - [keymetrics.io](https://keymetrics.io/) allows **2 free hosts** and is build on the top of [pm2](https://github.com/Unitech/PM2)
 - [node-monitor](https://github.com/lorenwest/node-monitor) with [monitor-dashboard](https://github.com/lorenwest/monitor-dashboard) is **open source**
 - [hummingbird](http://projects.nuttnet.net/hummingbird/) is **free**
 - [nodetime](http://nodetime.com/) allows **1 free agent**


