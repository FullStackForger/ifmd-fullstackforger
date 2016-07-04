# Tunneling - using EC2 as web proxy - complete guide

This is complete quite to tunneling and proxying for using EC2 Instance as a web proxy allowing you to direct web traffic through Amazon server.


## The what and the why


### Proxy

A proxy or proxy server is a hub computer processing requests. Proxy server serves as an intermediary between on machine and the Internet. Proxies are used for a number of reasons: to filter web content, to go around restrictions (including parental blocks), to screen downloads and uploads and to provide anonymity when surfing the Web.

### Tunnel

Wikipedia describes tunneling as using one protocol called the delivery protocol to encapsulate another payload protocol. That provides ability of using one protocol to carry a payload over an incompatible delivery-network. Another scenario would be providing secured path through an untrusted network or creating new path over firewall blocking some traffic.

### Proxy vs tunnel

The main difference between a proxy and a tunnel is the behavior.
A tunnel forwards requests and responses without modifying them.
A proxy adds own identification to requests via header. Additionally, proxy may cache responses or require proxy authentication.

## Real life example

Lets imagine following scenario.
```
.  [ computer ]
        |
   [ your ISP ]
        |   \
        |    \
        x   [ proxy server ]
        |    /
        |   /
  [ web server ]
```
So you are trying to access information from `web server`, however your ISP blocks the traffic. Good example would be trying to access US content that is not available in Europe or perhaps trying to access ThePirateBay site from UK. *ThePirateBay site is blocked by BRMI (British Recorded Music Industry). Access to it is law restricted so obviously you would do at your own risk.* Nevertheless, that's what proxying is about.

## EC2 Instance as a Web Proxy with Tiny Proxy

What you need is server with access to the resource. EC2 Instance with SSH access is probably the easiest way to go about it.
If you need full manual how to create and prepare your server on AWS check AWS guides. This guide assumes have one already. SSH to it with either SSH command if you are on POSIX system or use putty if you are Windows user.

### Installation

Installing on Ubuntu is very straightforward

```
sudo apt-get install tinyproxy
```

Installing TinyProxy on CentOS requires extra step

```
yum --enablerepo=epel  install tinyproxy
```

### Configuration

First, dind out your public IP address with  www.whatismyip.com site or if you in local terminal just type :

```
curl ipecho.net/plain
```

Edit config file
```
vim /etc/tinyproxy/tinyproxy.conf
```


Find “Allow” section and edit it so it’ll look like one bellow.
```
Allow 91.120.100.90
```

IF you are on dynamic IP you might want to remove or (better) comment that line. It will allow traffic on TinyProxy port from any IP address. Not the safest option but will work.

Alternatively you can allow connections from IP addresses from either one or many different groups.
If your dynamic addresses are in range `91.*.*.*` use CIDR (Classless Inter-Domain Routing) notation.
```
91.0.0.0/8
```
or for range `91.120.*.*` use:
```
91.120.0.0/16
```

You can also change mapped port in “port” section to lets say 8888
```
Port 8888
```

### Prevent memory leaks

Best way to prevent memory leaks is setting up a daily cron job that will restart the service.
Use the command `crontab -e` to edit the crontab file and add the following line:

```
0 2 * * * /etc/init.d/tinyproxy restart
```

### Security groups

Using EC2 as proxy server will also require configuring security group.
Similarly to IP configuration for TinyProxy you can use CIDR IP.
You could go ahead and create following rule:
* Type: `Custom TCP rule`
* Protocol: `TCP`
* Port Range: `8888`
* Source: `91.120.0.0/16`


### Proxy-ing a browser

To configure any browser, find connection settings and select `use proxy`, then paste public server IP address along with configured port `8888`.

### Even more security

In some cases you might not want to open additional 'proxy' port on the server. This is when tunnel comes handy.  

#### Tunnel from POSIX operating system

Connecting from POSIX systems is not difficult but requires you to type below command into your terminal command line replacing user name, server address and path to your .pem file. Bellow command will create a tunnel to EC2 server allowing to use local port `666` to connect to remote port `8888`.

```
sudo ssh ec2-user@ec2-user@ec2-xxx-xx-xxxx-xx -i ~/.ssh/aws_key.pem -L 666:localhost:8888 -N
```

Additionally you can create permanent alias. Add below line to `.bash_profile` file.

```
alias proxystart='ssh -L 3128:localhost:8888 -N -i ~/.ssh/aws_key.pem ec2-user@ec2-xxx-xx-xxxx-xx &'
```

Now run `proxystart` from the terminal to open tunnel and move process in the background.

**Note:**  
Remember that it won’t auto start with your system, so if you reboot your mac you will have to start manually.
