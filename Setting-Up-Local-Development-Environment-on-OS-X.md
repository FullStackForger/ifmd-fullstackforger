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

