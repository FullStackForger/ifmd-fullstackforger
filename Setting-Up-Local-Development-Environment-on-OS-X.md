# Setting up Local Development Environment on OSX (Mavericks 10.9)

This is step by step guide on how to set up local OSX Development Environment used at [Rusticode.com](http://rusticode.com)

[Back to Index](Index.md)

## Git 

## Mack Ports

Download and install pkg file from [mackports.org](https://www.macports.org/install.php)

## Node Package Manager ( NVM )
source: [github.com/creationix/nvm](https://github.com/creationix/nvm)

```
#!bash
git clone https://github.com/creationix/nvm.git ~/.nvm
cd ~/.nvm
git checkout `git describe --abbrev=0 --tags`
source ~/.nvm/nvm.sh
echo -e "\n# Activate nvm\nsource ~/.nvm/nvm.sh" >> ~/.bash_profile
```

## NodeJS

### Using Node 0.10.32
Install node using NVM 

```
nvm install 0.10.32
```

### Linking node 

From terminal open `sudo vim /usr/bin/node`, provide password if requested, press `[i]` to switch to `insert` mode and paste below code

```
if [[ "$NVM_BIN" ]]
then
  com="$NVM_BIN/node"
else
  com="/Users/Marek/.nvm/v0.10.32/bin/node"
fi
$com $@
#/Users/Marek/.nvm/v0.10.32/bin/node $@
```

Press `[esc]` to exit edit mode and type `:wq` to save and exit.

Make sure file has right privilages assigned
```
sudo chmod 755 /usr/bin/node
```

Create alias to make sure command always points to the right script
```
echo -e "\n# Alias to custom (nvm based) node script\nalias node="/usr/bin/node\"" >> ~/.bash_profile
```

Lastly source your `~/.bash_profile` executing below command from terminal
```
source ~/.bash_profile
```

### Node Packages

Install following packages
```
npm install -g grunt-cli slush http-server
```

Links do those npm packages
 - [Grunt](https://www.npmjs.org/package/grunt-cli)
 - [Slush](https://www.npmjs.org/package/slush)
 - [Http-Server](https://www.npmjs.org/package/http-server)

## MongoDB

To insall MondoDB folllow [Installing MongoDB with MacPorts on OS-X](Installing-MongoDB-with-MacPorts-on-OS-X.md)

## NginX

