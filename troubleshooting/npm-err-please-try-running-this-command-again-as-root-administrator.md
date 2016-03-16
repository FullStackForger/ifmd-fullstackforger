# `NPM err! please try running this command again as root/Administrator`

Sometimes when you try to install npm package globally, eg:

```bash
npm install express -g
```  
You can get an error like below:
```
Error: EACCES, mkdir '/usr/local/lib/node_modules/express'
npm ERR! { [Error: EACCES, mkdir '/usr/local/lib/node_modules/express']
npm ERR! errno: 3,
npm ERR! code: 'EACCES',
npm ERR! path: '/usr/local/lib/node_modules/express',
npm ERR! fstream_type: 'Directory',
npm ERR! fstream_path: '/usr/local/lib/node_modules/express',
npm ERR! fstream_class: 'DirWriter',
npm ERR! fstream_stack:
npm ERR! [ '/usr/local/lib/node_modules/npm/node_modules/fstream/lib/dir-writer.js:36:23',
npm ERR! '/usr/local/lib/node_modules/npm/node_modules/mkdirp/index.js:37:53',
npm ERR! 'Object.oncomplete (fs.js:107:15)' ] }
npm ERR!
npm ERR! Please try running this command again as root/Administrator.
```
Boom!

Nasty error occurs not only during installing express via npm package manager, but matter of fact, during installing other node packages too.

Error suggests solution.
```
NPM err! please try running this command again as root/Administrator
```

You are most likely to get that error if using Mac OS or linux. Main reason is that npm (Node Package Manager) requires “sudo” (root/Administrator) privileges. Running it as a regular user you don’t really have a write access to listed directory  `/usr/local/lib/node_modules` therefore new `express` directory can not be created at that location.

There is more than one method to go about this.

## Method 1: `sudo`

You can simply sudo yourself and run below command
```
sudo npm install express -g
```

Disadvantage is that you will have to sudo yourself every time you want to install package with “-g” (global) option

## Method 2: Group privileges

You can add yourself to the group that npm user belongs to.
Check the group running:
```bash
ls -al /usr/local/bin/npm
```

Add yourself to the group by running

```bash
usermod -a -G your_user_name desired_group_name
```

Big disadvantage of that method is that it will give you access to everything that "desired group" has access to.

## Method 3: make `npm` a `sudoer`

You might be better off by granting access to `npm` more permanently without the need of typing a password every time you run `sudo npm`.

To do that can modify `/etc/sudoers` file. Entry we are about to add should follow its general syntax:

```bash
user_name  host_name=command_location
```

And so before you proceed login as root

```bash
sudo su
```

And then execute below command replacing “your_user_name” with your user name

```bash
echo "your_user_name  localhost=/usr/local/bin/npm" >> /etc/sudoers
```

Now `CTRL-D` to logout from root and `CTRL-D` one more time logout from your account.
Reopen terminal and try to install your express (or any other node package) with

```bash
sudo npm install -g package_name
```

Disadvantage of that method is `npm` has been granted `sudo` permissions.
It is not a problem if it is just you on the server, or mac but **musn't**
be used if there is more users.

## Method 4: make `npm` a `sudoer`

…and voilà! There you have it.
