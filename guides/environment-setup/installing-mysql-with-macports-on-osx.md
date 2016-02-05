# Installing MySQL with MackPorts on OSX

## Preparation
```
sudo port -d selfupdate
```
## NginX

Check Installing NginX on OSX guide.

### Sources:

## MySQL

### Installation and configuration

#### Install mysql and mysql server
```
sudo port install mysql56 mysql56-server
```

#### Add mysql to system $PATH. 
You can do it by executing either of below commands
```
# export PATH=$PATH:/opt/local/lib/mysql56/bin 
sudo port select mysql mysql56
```

#### Setup main databsae:
```
sudo -u _mysql mysql_install_db 
sudo chown -R _mysql:_mysql /opt/local/var/db/mysql56/ 
sudo chown -R _mysql:_mysql /opt/local/var/run/mysql56/ 
sudo chown -R _mysql:_mysql /opt/local/var/log/mysql56/ 
```

#### Setup root password
```
/opt/local/lib/mysql56/bin/mysqladmin -u root -p password 
```
#### Autoload MySQL after reboot
```
sudo port load mysql56-server
```
### Starting and stopping MySQL server 

Start MySQL with:
```
sudo port load mysql56-server
```

Stop MySQL with:
```
sudo port unload mysql56-server
```

### Tweaking `.bash_profile` for handy aliases
```
# mysql start, stop, restart aliases
alias mysql-start="sudo port load mysql56-server"
alias mysql-stop="sudo port unload mysql56-server"
alias mysql-restart="mysql-stop; mysql-start;"
```
### Issues:
If you having problems with connection you might want to check out this troubleshooting page:
_Check: Warning: mysqli_connect(): (HY000/2002): No such file or directory_
<!-- [TODO] link to: #Warning: mysqli_connect(): (HY000/2002): No such file or directory -->


### Sources:
 - [mackports.org guide](https://trac.macports.org/wiki/howto/MySQL) 
