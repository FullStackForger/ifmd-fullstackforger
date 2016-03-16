# Strider CI on AWS ec2 <br />Part 2: Automation

This chapter will walk you through using web hooks to automate Strider deployments. That setup will allow Github to ping Strider server every time new commits are pushed to a project repository. Strider then can run tests and deploy apps to target environments.

## Git setup
Before you start using Strider with Github or Bitbucket, you have to do couple of things.

### GIT user config

Configure git username and email locally
```
git config --global user.name your username
git config --global user.name youremail@gmail.com
```

**Note:** even though CI server doesn't push anything back it might be good idea to set up separate github account for deployments.

## Github Webhooks

### Public github email

Public email is required so log into Github and navigate to `profile > public email` and change `Don't show my email` to address you setup in Strider CI account.

**Note:** if you don't like keeping your email public, you can undo it once you Github Strider setup is complete.

### App registration

Register your application following instructions from registration page.
When you get to callback address, use something like:
```
http://localhost:3000/auth/github/callback
```
**Note:** Remember to replace `localhost:3000` with your CI server url address. Also, this step is not required if you run strider locally.

Keep page open, you will use App Client ID and App Client Secret in a minute.

### PM2 startup Update

Now you can modify pm2 startup described in [Part 1: Strider Setup](./part-1-strider-setup), by additional variables `PLUGIN_GITHUB_APP_ID` and `PLUGIN_GITHUB_APP_SECRET`.

First stop and remove currently running `strider` app using PM2 CLI.
```
pm2 delete strider
```

```bash
NODE_ENV="production" \
SERVER_NAME="http://ci.your-server-name.com" \
STRIDER_CLONE_DEST="/home/ubuntu/strider-builds/" \
PLUGIN_GITHUB_APP_ID="your_app_client_id" \
PLUGIN_GITHUB_APP_SECRET="your_app_client_secret" \
pm2 start strider
```

Remember to run `pm2 save` to persist configuration.

### Verification

Open strider ci app in your browser. Log in and add one of the GIT project to your deployment projects. Once you have done that you can go back to Github.

Navigate to your project settings. In my case it would be `https://github.com/your_account/project_name/settings`. Then open `Webhooks & services` tab and you will see that Strider already created webhook for the repository, when you added it to your CI projects.

<!-- todo: image -->

From now on, every time you push committed changes to remote, Github will notify CI server and Strider will run tests and attempt to deploy application to the App Server.


## Bitbucket Webhooks

Bitbucket process is very similar with, the only change you will have to make is update environment variables. So if you use both Bitbucket and Github startup script would look like below.

If your server runs on address other than localhost:3000, set it via environment variable `SERVER_NAME`.

```bash
SERVER_NAME="https://appxample.com" npm start
```

## Creating your own bitbucket app

Open bitbucket settings page and navigate to `oauth` tab. You can also do it directly via url: `https://bitbucket.org/account/user/[your-username]/api`. Then click `Addconsumer`.

Use `http://ci.your-server-server.com/auth/bitbucket/callback`

http://ci.indieforger.com/auth/github/callback

Then set the env variables `PLUGIN_BITBUCKET_APP_KEY` and
`PLUGIN_BITBUCKET_APP_SECRET` to the values given you.

```bash
NODE_ENV="production" \
SERVER_NAME="http://ci.your-server-name.com" \
STRIDER_CLONE_DEST="/home/ubuntu/strider-builds/" \
PLUGIN_BITBUCKET_APP_KEY="your_app_client_id" \
PLUGIN_BITBUCKET_APP_SECRET="your_app_client_secret" \
PLUGIN_GITHUB_APP_ID="your_app_client_id" \
PLUGIN_GITHUB_APP_SECRET="your_app_client_secret" \
pm2 start strider
```
Remember to run `pm2 save` to persist configuration.
