# Tunneling - using EC2 as web proxy - complete guide

This is complete quite to tunneling and proxying for using EC2 Instance as a web proxy allowing you to direct web traffic through Amazon server.


## The what and the why

Tunneling through EC2 Instance is not a new topic and there are tones of resources showing how to do it in one or the other way for “a” platform.

Wikipedia describes tunneling as using one protocol called the delivery protocol to encapsulate another payload protocol. That provides ability of using one protocol to carry a payload over an incompatible delivery-network. Another scenario would be providing secured path through an untrusted network or creating new path over firewall blocking some traffic.

So in lame terms it means access to places you wouldn’t be able to access without the tunnel. For example if you are in Europe and you can’t open US site you can do it through the tunnel. Another example would be accessing ThePirateBay from UK. Site is blocked by BRMI (British Recorded Music Industry). Access to it is law restricted so obviously you would do at your own risk. Nevertheless, that's what tunneling is for.


## Using  EC2 Instance as a Web Proxy

EC2 Instance with SSH access
If you need full manual how to create and prepare your server on AWS check AWS guides. I am assuming you already have one and you are able to SSH to it. I will not go through complete server prep here but here is check list to go through:

### Log in to the server with SSH
* For Windows: EC2 – how to access AWS via SSH with putty
* For Linux/IOS: EC2 – ssh configuration with .pem keys

### Install TinyProxy
Remember to sudo yourself. Doesn’t matter which platform you have to have root access

```
sudo su
```

Installing on Ubuntu is very straightforward

```
sudo apt-get install tinyproxy
```

Installing TinyProxy on CentOS requires extra step

```
yum --enablerepo=epel  install tinyproxy
```

### Configure Tiny Proxy

Use your favorite editor to edit config file

```
vim /etc/tinyproxy/tinyproxy.conf
```

Find out your public IP address with  www.whatismyip.com site or if you in local terminal just type :

```
curl ipecho.net/plain
```

Find “Allow” section and edit it so it’ll look like:

```
Allow 192.168.1.1
```

IF you are on dynamic IP you might want to remove or (better) comment that line. It will allow traffic on TinyProxy port from any IP address. Not the safest option but will work.

You can also change mapped port in “port” section to lets say 8888

```
Port 8888
```

### Connect to the server
Connecting from IOS is not difficult but requires you to type below command into your terminal command line replacing user name, server address and path to your .pem file with the right data.

```
sudo ssh ec2-user@ec2-user@ec2-xxx-xx-xxxx-xx -i ~/.ssh/aws_key.pem -L 666:localhost:8888 -N
```

Best will be to create alias using your .bash_profile file, so open it with your favourite terminal editor like pico

```
vim ~/.bash_profile
```

And paste below line aliasing above command to a simpler one like  proxystart

```
alias proxystart='ssh -L 3128:localhost:8888 -N -i ~/.ssh/aws_key.pem ec2-user@ec2-xxx-xx-xxxx-xx &'
```

Re-open terminal and you will be able to start your proxy connection by typing proxy straight from the command line. It will open connection and set the whole process in background.

**Note:**  
Remember that it won’t autostart with your system, so if you reboot your mac you will have to start manually.

### Configure your browser

With proxy set up on your EC2 instance it is possible to configure your machine to connect to Internet with all protocols through the proxy however the best way might be just to set one browser to use tunnel and leave everything else as it is. Personally I am fan of that approach. I have few browser installed out of which only Firefox is connecting through web proxy. To configure that you would have to open Network settings in your browser and select ‘use proxy’ as shown below using port 3128 and address localhost as we already tried it. Here is the image how it could look for Firefox

![Firefox configuration settings](http://indieforger.com/wp-content/uploads/2016/02/Firefox-proxy-setup.png)
