# Strider CI on AWS EC2 <br/>Part 3: Continuous Deployment via SSH

## AWS Environment

### Scenario

Below diagram illustrates  simple scenario where Strider is installed on one server instance and used to deploy projects to Server 1 and Server 2 via SSH.

```
`   [ CI Server ]
          |
       ( SSH )
          |        
  [ App Server 1 ]
```

### Preparation

Before we start we need at least one project to deploy. If you followed [part 2](./part-2-automation) you should already have it.

If, however, you haven't done it yet, do it now.  First authorize Strider to your CVS account (Github or Bitbucket). It will then list all your projects. Pick and add one and you good to go.

**Note:** it is good idea to start with small project, which will allow you to test deployments super fast. If you don't have one use [micro-pack](https://github.com/indieforger/micro-pack). It is super tiny server running on port 8080 with one dummy mocha test. You can go ahead and create simple project if you don't have now or just clone and use [micro-pack](https://github.com/indieforger/micro-pack) for now.

### Access configuration

In [Part 1: Strider Setup](./part-1-strider-setup) and [Part 2: Automation](./part-2-automation) I showed you how to setup CI server and configure Strider automation. Now we need to make sure Strider CI server can access target server we want to deploy application to.

As described in [how to ssh from one ec2 instance to another](../../aws/how-to-ssh-from-one-aws-ec2-instance-to-another) there is more than one way to configure ssh access. Strider as of now doesn't support .pem key based authentication. Instead, it generates SSH keys allowing SSH based deployment. We will use those.

#### Server setup

Lets assume you don't have target app server yet, so lets spin EC2 Ubuntu 14.04 LTS 64bit instance and SSH to it using using .pem key.
Then reuse configuration script from [Part 1: Strider Setup](./part-1-strider-setup) to install dependencies.

```
curl -L https://gist.githubusercontent.com/indieforger/20e345cf9dff308a7392/raw/setup-node-webserver.sh | bash
```

If you need MongoDB go ahead and install it but remember that **it is NOT good practice to go with single MongoDB**. Refer to MongoDB official docs for more details.
```
curl -L https://gist.githubusercontent.com/indieforger/20e345cf9dff308a7392/raw/install-mongodb.sh | bash
```

#### Deployment user

As always it is good idea to create deployment user.
Example below will create local user called `strider` with enabled SSH access granted to our CI server.

```
# add user without password and not interactive
sudo adduser --disabled-password --gecos "" strider
# enable ssh authorization
sudo mkdir -p /home/strider/.ssh
sudo touch /home/strider/.ssh/authorized_keys
sudo chown -R strider:strider /home/strider/.ssh
sudo chmod 0700 /home/strider/.ssh
sudo chmod 0600 /home/strider/.ssh/authorized_keys
```

For now `strider` user is all we need. In the next chapter we will also use it to manage node processes.

## Strider SSH-Deploy plugin

### Installation

Strider SSH-Deploy plugin installation should be done by default.
If it is not, however, you can do it from `admin > plugins` section.

### Activation

Strider is built around plugin based deployment individually configured for each project.
It means plugins have to be manually activated. It can be done from project configuration screen.

To do so navigate to `admin > projects > your project > configure > plugins`.

Then, drag SSH-Deploy from Available Plugins to Active Plugins. It will enable additional menu item called "SSH Deploy" in the menu project menu panel located on the left hand side of the screen.

### Configuration

#### SSH keys

Earlier, I asked you to stay logged in to your App Server for a reason.

In this step you have to copy Public SSH Key available from project settings page and paste it into `/home/strider/.ssh/authorized_keys` file located on app server.

It will allow Strider Server to authenticate with ssh to deploy application files.

#### Deployment settings

There are different ways to go about deployment.

You could, for example, configure your CI server to ssh to App Server and clone/pull the project source at desired location, configure it, run some tests and if successful you could then start/restart the app.

Another method is to run all of those tests locally and if successful compress build files and send them to the app server and then run some tests and if successful you could then start/restart the app.

For now lets just configure CI to transfer tested build. To do that log in to Strider web client. Next go to the dashboard and click wrench icon to open settings page. Go to `SSH Deploy` plugin tab and and turn on `Transfer bundle` checkbox.

## Deploy

It is final step in this chapter.

If everything went ok you should be able to deploy to App Server with one click.
Go ahead and deploy your project.

You will see all the output in the deployment console.
If you want to verify deployed files manually, ssh to app server and check `/home/users/strider` directory. Project build should be there.

## What next

In next chapter we will look at:
- configuring app server auto start application after system reboot
- configuring deployment script to restart application when deployment is finished

## Great sources
https://futurestud.io/blog/strider-continuous-deployment-to-any-server-via-ssh
