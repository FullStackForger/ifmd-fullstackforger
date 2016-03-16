# How to access AWS EC2 instance via SSH with putty

This is article explains what is private / public key pair and shows how to generate private .ppk key for Putty in order to access AWS instance with Putty SSH client. It has been written with Windows users in mind.


## Public / private key pair on AWS

Any SSH server that is visible in Internet should be used with public authentication key instead of password. Combination of public / private key authentication is additional security level. Once public and a private “key”  is generated, private key is stored on the machine user is logging from, while the public key is kept in the .ssh/authorized_keys file on one or more computers user will be logging int to.

For Windows users  Putty SSH client  requires private key being generated from public key, which is .pem file that can be downloaded from the server. Authentication mechanism is quite simple. Server uses the public key to “lock” messages and those can be  “unlocked” by private key. That mechanism prevents from all types of attacks, or interfering established session. Additional step you can make to increase security would be storing the private key in a passphrase-protected format. That protects server from unauthorized access even if client computer is stolen giving enough time to change security before thief breaks pass phrase.

Public key authentication “must have” solution in today’s world. Not only it increases security but also might speed up authentication process with your server if you are not bothered about leaving a private key unprotected on your hard disk. You could use it to secure automatic log-ins – as part of a network backup, for example.

### Generating public / private key pair

To generate a key pair:

- Open the Amazon EC2.
- Click Key Pairs in the Navigation pane.
- The console displays a list of key pairs associated with your account.
- Click Create Key Pair.
- The Key Pair dialog box appears.
- Enter a name for the new key pair in the Key Pair Name field and click Create.
- You are prompted to download the key file.
- Download the key file and keep it in a safe place. You will need it to access any instances that you launch with this key pair.

**Important!**

 Remember to store you private key. If you will lose it you will have to generate it again and recreate all the instances using it’s public key match.

### First login to AWS EC2 instance with Putty

Before you proceed make sure you have downloaded and PuTTY client and PuTTY Key Generator  available on the same download page. Once you got them follow bellow steps.

- Launch PuTTYgen and then load your private EC2 key.
- Click save private key and now if you don’t mind storing it without a password (do it if you want to get quicker access to your server)
- Now launch PuTTY client and create new SSH session for your EC2 instance host name or IP address (if you are using Elastic IPs)
- Navigate to Connection/SSH/Auth and under “private key for authentication” set location to your .ppk file.
- Save session and try to log in.
- If  your instance is build on AWS Linux AMi log in with user: ec2-user  if you are using Ubuntu AMI log in with user: ubuntu
- If you followed all steps you should see screen similar to this one

```
login as: ec2-user
Authenticating with public key "imported-openssh-key"
Last login: Wed Apr  3 10:18:15 2013 from 93-96-235-39.zone4.bethere.co.uk

       __|  __|_  )
       _|  (     /   Amazon Linux AMI
      ___|\___|___|

https://aws.amazon.com/amazon-linux-ami/2013.03-release-notes/
There are 2 total update(s) available
Run "sudo yum update" to apply all updates.
[ec2-user@ip-10-210-141-174 ~]$
```
