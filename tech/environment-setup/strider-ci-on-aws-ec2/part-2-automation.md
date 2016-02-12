# Strider CI on AWS ec2
# Part 2: Automation

## Prepare GITHUB

Before you start using Strider with Github or Bitbucket, you have to do couple of things.

### Public github email

Public email is required (if you don't want to do it permanently you can undo it once you done with setup)

**Important:** there are 2 places you can do it. The one you after is changing:
`profile > public email > Don't show my email` to address you going to use from your CI account.

### Github user config

Configure git username and email locally
```
git config --global user.name your username
git config --global user.name youremail@gmail.com
```

**Note:** even though CI server doesn't push anything back it might be good idea to set up separate github account for deployments

### App registration


Register your application following instructions from registration page.
When you get to callback address, use something like:
```
http://localhost:3000/auth/github/callback
```
**Note: ** Remember to replace `localhost:3000` with your CI server url address. Keep page open, you will use App Client ID and App Client Secret in a minute.

**Note:** This step is not required if you run strider locally

### PM2 startup Update

Now you can modify pm2 startup described in [Part 1: Strider Setup](./part-1-strider-setup), by additional variables `PLUGIN_GITHUB_APP_ID` and `PLUGIN_GITHUB_APP_SECRET`.

```bash
NODE_ENV="production" \
SERVER_NAME="http://ci.indieforger.com" \
STRIDER_CLONE_DEST="/home/ubuntu/strider-builds/" \
PLUGIN_GITHUB_APP_ID="your_app_client_id" \
PLUGIN_GITHUB_APP_SECRET="your_app_client_secret" \
pm2 strider    
```

Remember to run `pm2 save` to persist configuration.

## Automation with Github Webhooks

Navigate to your project settings. In my case it would be `https://github.com/indieforger/micro-pack/settings`. Then open `Webhooks & services` tab and you will see that Strider already created webhook for the repository, when you add it to your CI projects.

Every time you push committed changes to remote Github will notify CI server and Strider will run tests and attempt to deploy application to the App Server.
