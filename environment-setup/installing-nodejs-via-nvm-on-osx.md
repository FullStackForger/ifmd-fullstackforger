# Installing NodeJS via NVM on OSX

## Installing NVM

> source: [github.com/creationix/nvm](https://github.com/creationix/nvm)

```
#!bash
git clone https://github.com/creationix/nvm.git ~/.nvm
cd ~/.nvm
git checkout `git describe --abbrev=0 --tags`
source ~/.nvm/nvm.sh
echo -e "\n# Activate nvm\nsource ~/.nvm/nvm.sh" >> ~/.profile
echo -e "\n# Bash completion for nvm\n[[ -r $NVM_DIR/bash_completion ]] && . $NVM_DIR/bash_completion" >> ~/.profile
```

## Installing node via NVM

Install and use node using NVM 

```
nvm install node
nvm use node
```

## Linking node (optional)

If you don't want to be forced to use `nvm use [node_version]` after every reboot you might want to add that into `~/.bash_profile` file. Alternatively you might create executable script that will detect node version executing default if there is none in use.

### Method 1: Updating _.bash\_profile_ or _.profile_

```
echo -e "\nnvm use node" >> ~/.profile
```

### Method 2: Linking npm and node binaries

#### Link node

From terminal open 
```
sudo vim /usr/bin/node
```

Provide password if prompted, press `[i]` to switch to `insert` mode and paste below code.

```
if [[ "$NVM_BIN" ]]
then
  com="$NVM_BIN/node"
else
  com="/Users/Mark/.nvm/v6.9.1/bin/node"
fi
$com $@
#/Users/Mark/.nvm/v6.9.1/bin/node $@
```

Press `[esc]` to exit edit mode and type `:wq` to save and exit.

Make sure file has right privilages assigned
```
sudo chmod 755 /usr/bin/node
```

#### Link Node Package Manager (NPM)

From terminal open 
```
sudo vim /usr/bin/npm
```

Provide password if prompted, press `[i]` to switch to `insert` mode and paste below code.

```
if [[ "$NVM_BIN" ]]
then
  com="$NVM_BIN/npm"
else
  com="/Users/$USER/.nvm/v6.9.1/bin/npm"
fi
$com $@
#/Users/$USER/.nvm/v6.9.1bin/npm $@
```

Press `[esc]` to exit edit mode and type `:wq` to save and exit.

Make sure file has right privilages assigned
```
sudo chmod 755 /usr/bin/npm
```