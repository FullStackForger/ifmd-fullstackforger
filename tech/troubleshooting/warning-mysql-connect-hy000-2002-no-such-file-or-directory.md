#Warning: mysqli_connect(): (HY000/2002): No such file or directory ...

If you getting this error there is good chance you are trying to run mysql on OSX. If yes here is just the solution. 

Error is caused because MySQL socket is not configured properly. 

Error message is not intuitive at all.

Open MySQL config.
_Example uses default config created during installation with macports._
```
sudo vim /opt/local/etc/mysql56/my.cnf
```

And paste below lines:

```
[mysqld]
socket=/tmp/mysql.sock

[client]
socket=/tmp/mysql.sock
```

Restart mysql with:
 
```
sudo port load mysql56-server
sudo port unload mysql56-server
```