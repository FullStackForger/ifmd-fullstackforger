# Useful Development OSX Tools and Applications

Below is list of useful tools, which is used as a **Development Toolkit** at [rusticode.com](http://rusticode.com)

[Back to Wiki Homepage](Home.md)

### IntelliJ IDEA

#### IDEA 13.1

IDEA 13.1 requires installing [Java SDK 1.6](http://support.apple.com/kb/DL1572)

IDEA 13.1 [download](https://confluence.jetbrains.com/display/IntelliJIDEA/Previous+IntelliJ+IDEA+Releases)

### MuCommander

Download and install pkg file from [www.mucommander.com](http://www.mucommander.com/)

### Robomongo 

Shell-centric cross-platform open source MongoDB Admin GUI

Download and install pkg file from [robomongo.org](http://robomongo.org/)


### iTerm2 

iTerm2 is a replacement for Terminal and the successor to iTerm.

Download and install pkg file from [iterm2.com](http://iterm2.com/)


### FileZilla

Download and and install from [official site](https://filezilla-project.org/download.php?type=client)

### Sublime (version 2.2)

#### Usage 

Used mainly for Markdown Documents editing, quick look into a folder with terminal command `subl .`, previewing text files

#### Editor Installation

Download and install sublime from [official page](http://www.sublimetext.com/3)

#### Plugin Installation

Then follow steps from below link to install Sublime Package Control 
https://sublime.wbond.net/installation

Open console with `[crl + ``]` and paste installation python code from 
the link above.

#### Markdown Plugins

To open isntallation palette hit `cmd + shift + p` and type `install package`.

To install plugin manually follow instruction from plugin page.
 - [Markdown Editing](https://sublime.wbond.net/packages/MarkdownEditing)
 - [Markdown Preview](https://sublime.wbond.net/packages/Markdown%20Preview)

#### Enable `subl` from command line

To be able to run Sublime from command line create a symlink

```
echo -e "\n# Sublime 2.x alias" >> ~/.bash_profile
echo "alias subl=\"/Applications/Sublime\ Text\ 2.app/Contents/SharedSupport/bin/subl\"" >> ~/.bash_profile
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


### Better Snap Tool

Shortcuts to configure

```
Cmd + alt + opt + {key}
```

Keys:
```
Y U I < top -left -center -right
H J K < middle -left -center -right
N M , < bottom -left -center -right
SPACE < center
```

### Slowy 

Real-world connection simulator and bandwidth limiter 

Download and install from [slowy site](http://slowyapp.com/download)

