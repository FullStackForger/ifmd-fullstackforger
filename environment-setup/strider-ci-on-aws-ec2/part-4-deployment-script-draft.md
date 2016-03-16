# Part 4: Deployment script

> **Warning:** This is recovered file. It might not be complete. Feel free to report an issue if you found one.

> todo: two ways: 1 - git clone, 2 - tar.gz upload
> last example Strider to ssh, this example strider to run scripts

## NodeJS process manager

> todo: there are many managers, why pm2 is good: (battle tested)

> todo:  in last example we create strider user, lets use it as process manager on app server

Log in as `strider`

```
sudo su strider
```

todo: Execute bellow commands to setup npm

```
mkdir ~/.npm-global
npm config set prefix '~/.npm-global'
printf "\nexport PATH=~/.npm-global/bin:$PATH\n" >> ~/.profile
source ~/.profile
npm install -g npm pm2
```

Now run pm2.
```
pm2 status
```

You will see:
```
┌──────────┬────┬──────┬─────┬────────┬─────────┬────────┬────────┬──────────┐
│ App name │ id │ mode │ pid │ status │ restart │ uptime │ memory │ watching │
└──────────┴────┴──────┴─────┴────────┴─────────┴────────┴────────┴──────────┘
 Use `pm2 show <id|name>` to get more details about an app
```

> todo: note about sourcing .profile

### Auto start PM2 after reboot

> todo:  modify chapter (copied from part 1), modify script to point to pm2 absolute location

PM2 comes with `startup` command that will help you to auto start pm2 start after system reboot.

```bash
pm2 startup ubuntu
```

It should output something like below, so just follow these instructions.
```
# [PM2] You have to run this command as root. Execute the following command:
#      sudo su -c "env PATH=$PATH:/usr/bin pm2 startup linux -u ubuntu --hp /home/ubuntu"
```
> source: https://futurestud.io/blog/pm2-restart-processes-after-system-reboot

Now you should be able to restart the server.
```
sudo reboot
```


## Configuring deployment script

todo: back to Strider web client, update configuration

```
source .profile
cd your_app_dir
pm2 stop your_app
pm2 start index.js --name="your-app"
```

## improvements

> todo:  why it is not finished (simplifued) demo,
> todo: if  single process route tables
> todo: if more than one use process manager -> link to (setting up small nodejs prod server),

## What next
> todo: notifications2
