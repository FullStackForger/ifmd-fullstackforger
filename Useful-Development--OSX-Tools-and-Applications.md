# Useful Development OSX Tools and Applications

Below is list of useful tools, which is used as a **Development Toolkit** at [rusticode.com](http://rusticode.com)

[Back to Index](Index.md)

### MuCommander

Download and install pkg file from [www.mucommander.com](http://www.mucommander.com/)

### Robomongo 

Shell-centric cross-platform open source MongoDB Admin GUI

Download and install pkg file from [robomongo.org](http://robomongo.org/)


### iTerm2 

iTerm2 is a replacement for Terminal and the successor to iTerm.

Download and install pkg file from [iterm2.com](http://iterm2.com/)


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



### Sequel Pro 

MySQL databse management for Mac OSX

Download and install pkg file from [www.sequelpro.com](http://www.sequelpro.com/)


