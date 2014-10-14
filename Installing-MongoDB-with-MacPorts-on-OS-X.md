#Installing MongoDB with MacPorts on OS-X 

1. **Install mongodb** with `sudo port install mongodb`
2. **Make a data directory** with `sudo mkdir -p /opt/local/var/db/mongodb_data`
3. **Make a logs directory** with `sudo mkdir -p /opt/local/var/log/mongodb/`
4. **Create configuration file** with `sudo pico /opt/local/etc/mongodb/mongod.conf`

 Enter the following if the file is blank and hit ctrl+x and save the file.

 ```
 # configuration file /opt/local/etc/mongodb/mongod.conf

 # Store data alongside MongoDB instead of the default, /data/db/
 dbpath = /opt/local/var/db/mongodb_data

 # Only accept local connections
 bind_ip = 127.0.0.1

 # Running as daemon
 fork = true

 # Take log
 logpath = /opt/local/var/log/mongodb/mongodb.log
 logappend = true
 ```

5. **Create startup alias** for your terminal profile so that you can run `mongostart` and `mongostop` to manually start and stop the mongodb instance.

 At the Terminal, enter `sudo pico ~/.profile` and add the following to the end of the file. When done hit ctrl+x and save the file:

 ```
 alias mongostart="sudo mongod -f /opt/local/etc/mongodb/mongod.conf"
 
 mongostop_func () {
    local mongopid=`less /opt/local/var/db/mongodb_data/mongod.lock`;
    if [[ $mongopid =~ [[:digit:]] ]]; then
        sudo kill -15 $mongopid;
        echo mongod process $mongopid terminated;
    else
        echo mongo process $mongopid not exist;
    fi
 }
 
 alias mongostop="mongostop_func"
 ```

6. **Start MongoDB** by typing `mongostart` at the Terminal and hitting return (to stop, type `mongostop` and hit return).
7. **Test that MongoDB** is running by visiting `http://localhost:28017/` in a web browser. You should see a dashboard summarizing the running instance.