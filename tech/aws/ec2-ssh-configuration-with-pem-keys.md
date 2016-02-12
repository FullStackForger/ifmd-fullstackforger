# EC2 – SSH configuration with .pem keys

This article explains how to connect to EC2 server using your ssh command and how to configure ssh to avoid passing additional parameters every single time.


## Connecting to Amazon EC2
Before you get connected you will have to download Amazon EC2 .pem file and copy it into your secured location. Good place to store the key would be your local ssh folder: `/Users/your_user_name/.ssh/ec2.pem` or `/home/your_user_name/.ssh/ec2.pem`

To test ssh connection you would need to run below command from your console, replacing ec2.server.name.com with your server name.

```
ssh -i ~/.ssh/ec2.pem ec2-user@ec2.server.name.com
```

If you got connected certification worked as it should. Otherwise make sure you are running this command from the right folder and you passing right location to your file. Also remember that EC2 standard user is not root but ec2-user.

## Configuring shortcut for even quicker access

Open or create ~/.ssh/config file using your favorite editor like pico, nano or vim.

```
pico ~/.ssh/config
```
and then paste following line replacing my.host with the id you want to use and location to .pem file with correct one.


```
Host my.host

HostName ec2.server.name.com
User ec2-user
IdentityFile "~/.ssh/ec2.pem"
```

Next save config changes and set right permissions of your PEM file to 700 to (1) protect it from unauthorized eyes and (2) to give your ssh client exactly what  is required.

```
chmod 700 ~/.ssh/ec2.pem
```

You are ready to go. Just type:

```
ssh my.host
```
