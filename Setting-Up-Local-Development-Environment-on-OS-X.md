# Setting Local Development Environment OSX ( Mavericks 10.9 )

[Back to Index](Index.md)

## Environment Prerequisites

### Mack Ports

Download and install pkg file from [mackports.org](https://www.macports.org/install.php)

### Node Package Manager ( NVM )
source: [github.com/creationix/nvm](https://github.com/creationix/nvm)

```
#!bash
git clone https://github.com/creationix/nvm.git ~/.nvm
cd ~/.nvm
git checkout `git describe --abbrev=0 --tags`
source ~/.nvm/nvm.sh
echo -e "\n# Activate nvm\nsource ~/.nvm/nvm.sh" >> ~/.bash_profile
```

### NodeJS
Install node using NVM 

nvm install 0.10.32

### MongoDB
To insall MondoDB folllow 
[Installing MongoDB with MacPorts on OS-X][installing-mongo]
[installing-mongo]: Installing-MongoDB-with-MacPorts-on-OS-X.md

### NginX


## Development toolkit (useful tools)

### MuCommander

Download and install pkg file from [www.mucommander.com](http://www.mucommander.com/)

### Sublime (version 3)

#### Usage 

Used mainly for Markdown Documents editing, quick look into a folder with terminal command `subl .`, previewing text files

#### Installation

Download and install sublime from [official page](http://www.sublimetext.com/3)

Then follow steps from below link to install Sublime Package Control 
https://sublime.wbond.net/installation

#### Installing plugins 

To install plugin open below links, scroll down to installation section and follow all the steps
 - [Markdown Editing](https://sublime.wbond.net/packages/MarkdownEditing)
 - [Markdown Preview](https://sublime.wbond.net/packages/Markdown%20Preview)

#### Enable `subl` from command line

To be able to run Sublime from command line create a symlink

```
echo -e "\n# Sublime alias" >> ~/.bash_profile
echo "alias subl=\"/Applications/Sublime\ Text.app/Contents/SharedSupport/bin/subl\"" >> ~/.bash_profile
```

To open file type 

```
subl some_file_name.txt
```

To open whole directory 

```
subl .
```
