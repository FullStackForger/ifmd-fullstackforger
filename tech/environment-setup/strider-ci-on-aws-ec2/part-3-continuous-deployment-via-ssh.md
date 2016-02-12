# Strider CI on AWS EC2
# Part 3: Continuous Deployment via SSH

## AWS Environment

### Scenario

Below diagram illustrates  simple scenario where Strider is installed on one server instance and used to deploy projects to Server 1 and Server 2 via SSH.

```
`
            [ CI Server ]
                  |
               ( SSH )
                  |
        +---------+--------+
        |                  |
[ App Server 1 ]   [ App Server 2 ]

```

### Preparation

Before we begin add a project to your Strider CI from either Github or Bitbucket.
You can do it by authorizing Strider to your CVS account. It will then list all your projects and you good to go.
Just pick and add one.

**Note:** it is good idea to start with super tiny project, which will allow you to test deployments super fast. If you don't have one use [micro-pack](https://github.com/indieforger/micro-pack). It is super tiny server running on port 8080 with one dummy mocha test.

### Access configuration

As described in [how to ssh from one ec2 instance to another](../../aws/how-to-ssh-from-one-aws-ec2-instance-to-another) there is more than one way to configure ssh access. Strider as of now doesn't support .pem key based authentication. Instead it generates SSH keys allowing SSH based deployment. We will use those.

#### Server setup

Lets start with configuring application server, so first ssh to app server using .pem key.
Then reuse script from part 1 to setup node environment with some dependencies.
```
curl -L https://gist.githubusercontent.com/indieforger/20e345cf9dff308a7392/raw/setup-node-webserver.sh | bash
```

#### Deployment user

It is good idea to create deployment user. Example below uses system user `strider` to deploy and run application on App Server.

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

#### Enabling node and pm2

Log in as `strider`
```
sudo su strider
```

Then run bellow commands to setup npm
```
mkdir ~/.npm-global
npm config set prefix '~/.npm-global'
printf "\nexport PATH=~/.npm-global/bin:$PATH\n" >> ~/.profile && source ~/.profile
```

And install required node packages
```
npm install -g npm node-gyp
```

That's it for now but stay logged in to App Server for now, we will come back to it in a moment.


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

In this step you have to copy Public SSH Key available from project settings page and paste it into `/home/strider/.ssh/authorized_keys`, hence earlier, I asked you to stay logged in to your App Server.

####

## Deploy

If everything went ok you should be able to deploy to App Server with one click.


## Great sources
https://futurestud.io/blog/strider-continuous-deployment-to-any-server-via-ssh
