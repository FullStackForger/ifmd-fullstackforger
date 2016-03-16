tech/aws/how-to-ssh-from-one-aws-ec2-instance-to-another.md
# How to SSH from one AWS EC2 instance to another

There is more than one way to go about it. You can generate ssh-key on your CI server and authenticate on application server. You will surely want to do it if you don't use AWS.

With Amazon Web Services you can use provided .pem key associated with application server instances. That's the method we will tackle here first.

## SSH connection with .pem key

First copy .pem key used for authenticating to App Server overt to CI server.
Run `scp` command from your local machine passing.
```
# scp -i ~/.ssh/ci.pem ~/.ssh/prod.pem ubuntu@111.222.333.444:/home/ubuntu/.ssh/prod.pem
#      (ci auth pem)    (local prod pem)  (ci hostname or addr)   (remote prod pem filename)
```

Then ssh yourself to CI Server.
```
# scp -i ~/.ssh/aws/ci.pem ubuntu@111.222.333.444
```

Create `~/.ssh/config` file with content as bellow modifying addresses to match yours.
```
Host prod.hostname.com
    User ubuntu
    HostName prod.hostname.com
    IdentityFile ~/.ssh/prod.pem
```

Save the file and test ssh connection
```
ssh prod.hostname.com
```

You should be able to successfully log in to your Application Server.

**Note:** you should configure each application server that way, regardless of whether you have one or more .pem authentication files.
