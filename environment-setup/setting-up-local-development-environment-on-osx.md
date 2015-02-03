# Setting up Local Development Environment on OSX (Mavericks 10.9)

This is step by step guide on how to set up local OSX Development Environment used at [Rusticode.com](http://rusticode.com)

[Back to Wiki Homepage](Home.md)

## Bash setup

Follow below instruction to display Bash colors schemes, useful aliases and current bash version, when you open up the terminal. 

From terminal open 
```
vim ~/.bash_profile
```

Press `[i]` for `editing` mode and paste below code.

```
# Color scheme for bash
export TERM="xterm-color"
PS1='\[\e[0;33m\]\u\[\e[0m\]@\[\e[0;32m\]\h\[\e[0m\]:\[\e[0;34m\]\w\[\e[0m\]\$ '
alias ls="ls -G"
alias npmls="npm ls --depth=0"
alias lsa="ls -al"
alias lsg="ls -al | grep -i"
alias psg="ps aux | grep -i"

# Make grep to highlight matches
export GREP_OPTIONS='--color=auto'

# Display current shell location and version
echo ""
echo ">> \$SHELL: ................. $BASH"
echo ">> \$BASH_VERSION: .......... $BASH_VERSION"
echo ""
```

Press `[esc]` to exist `editing` mode and type `:wq` and confirm with Enter to save changes.

## XCode 

OS X Yosemite and Xcode 6 [installation guide](https://developer.apple.com/osx)

### XCode Command Line Tools

XCode Command Line Tools are required by mac-ports

```
xcode-select --install
```

## Mack Ports

Download and install pkg file from [mackports.org](https://www.macports.org/install.php)

From the console run `sudo port selfupdate` to update Mack Ports

## Wget

Run the following command:
```
sudo port install wget
```

## Bash and Bash Completion

Source: [link](https://trac.macports.org/wiki/howto/bash-completion)

__For convinience instructions are pasted below.__

From the terminal run below command to install bash completion first.
```
sudo port install bash
sudo port install bash-completion
```

Paste following to your `~/.profile` or `~/.bash_profile`

```
# MacPorts Bash shell command completion
if [ -f /opt/local/etc/bash_completion ]; then
    . /opt/local/etc/bash_completion
fi
```

Add /opt/local/bin/bash to the file /etc/shells, otherwise it will be impossible to use AppleScript to tell Terminal to execute scripts (like 'osascript -e "tell application \"Terminal\" to do script \"echo hello\""').

### Configure Terminal.app

You need to change the command Terminal.app uses to launch the shell in the preferences.

1. Menu > Preferences > Startup, "Shells open with:"
2. Select "Command" and enter /opt/local/bin/bash -l to switch to bash provided by MacPorts.
3. Menu > Preferences > Settings > Shell (per shell type), "Prompt before closing:"
4. Click "+" and add "bash" to the list of processes.
5. Close and reopen any terminal windows

### Configure iTerm2

You need to change the command iTerm2 uses to launch the shell in the appropriate profile in the preferences.

1. Menu > Preferences > Profiles tab
2. Select your profile, on the right switch to the General tab, see "Command"
3. Select "Command:" and enter /opt/local/bin/bash -l
4. Close and reopen any terminal windows

## Git 

Install Git using Mack Ports. Git bash completion will be added if you followed all s teps in **Bash and Bash Completion** section.

```
sudo port install git +svn +doc +bash_completion +gitweb
```

### Git color schemas

We will use [magicmonty/bash-git-prompt](https://github.com/magicmonty/bash-git-prompt)

__For convinience instructions are pasted below.__

Execute below code from the terminal.

```
cd ~
git clone https://github.com/magicmonty/bash-git-prompt.git .bash-git-prompt
echo -e "\n# Git bash color schemas"
echo -e ""
echo -e "source ~/.bash-git-prompt/gitprompt.sh" >> ~/.bash_profile
source ~/.bash_profile
```

### Git settings: aliases, color, core

Use up to date config available as [rusticode/git-config.sh](https://gist.github.com/rusticode/262c9a1245f2156f8946).

```
wget -O - https://gist.githubusercontent.com/rusticode/262c9a1245f2156f8946/raw | bash
git alias
```

Resources: some have been borrowed from [durn.com](http://durdn.com/blog/2012/11/22/must-have-git-aliases-advanced-examples/)

### Git user settings 

```
git config --global alias.alias "config --get-regexp ^alias\." | sort
#git config --global user.name "User Name"
#git config --global user.email username@email.com
```

## NodeJS - installing node via .nvm

Check [Installing node via NVM](./installing-nodejs-via-nvm-on-osx.md)


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

To insall MondoDB folllow [Installing MongoDB with MacPorts on OSX](installing-mongodb-with-macports-on-osx.md)

## NginX
To install NginX follow [Installing NginX on OSX with configuration for NodeJS](installing-nginx-on-osx-with-configuration-for-nodejs.md).
