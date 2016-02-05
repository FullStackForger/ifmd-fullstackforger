# Deployment from OSX to AWS

## Deployment script

Nice and useful [deploy](https://github.com/tj/deploy) is a minimalistic deployment script by TJ allows deployment from command line.

Checkout it's github [wiki](https://github.com/tj/deploy/wiki) for more information.

### Prequisites checklist


> **note:**
> Although it is recommended to create 'deployer' user,
> you can use your default user.

<!--
sources:
 - http://askubuntu.com/questions/246455/how-to-give-nopasswd-access-to-multiple-commands-via-sudoers
 - http://unix.stackexchange.com/a/80159

##### Deployer
```
sudo vim /etc/sudoers
```

Disable `requiretty` for the user
```
bob ALL = (root) /path/to/program
Defaults:username !requiretty
```
-->

- Log in to the server.
- Generate new key with `ssh-keygen`.
- If deploying from BitBucket use `.ssh/id_rsa.pub` as a deployment key [instructions](https://confluence.atlassian.com/display/STASH/SSH+access+keys+for+system+use)
    + If deploying from GitHub you need "machine user". For more information check github guide [Managing deploy keys](https://developer.github.com/guides/managing-deploy-keys/)
- Clone your repository on the server.
- Configure server environment.
- Start your application and test it to make sure it runs ok.
