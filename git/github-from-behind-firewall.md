
# Github from behind firewall

## Problem

I use github (obviously) a lot but since British Telecom decided to randomly
switch my neighbour's phone line with mine and IPhone hotspot with o2 doesn't
allow ssh connections from _github.com_. I do have a problem. No Github.
HTTP, HTTPS, SSH work ok though.

**How to access github from behind firewall or O2 hotspot proxy?**

## Troubleshooting

I started with trying to ssh to it directly, to verify I can use github via SSH.
```
ssh -vT git@github.com
```

Unfortunately I got an error: `Could not resolve hostname github.com`.
```
OpenSSH_6.9p1, LibreSSL 2.1.8
debug1: Reading configuration data /Users/Forger/.ssh/config
debug1: Reading configuration data /etc/ssh/ssh_config
debug1: /etc/ssh/ssh_config line 21: Applying options for *
ssh: Could not resolve hostname github.com: nodename nor servname provided, or not known
```

Ooops!

Same error I get when I try to clone any repository or push to an existing one.

## Solution

SSH works ok, so I logged in to my aws server and run `ssh -vT` command again.
```
OpenSSH_6.6.1, OpenSSL 1.0.1f 6 Jan 2014
debug1: Reading configuration data /etc/ssh/ssh_config
debug1: /etc/ssh/ssh_config line 19: Applying options for *
debug1: Connecting to github.com [192.30.253.112] port 22.  <--- what we need!
debug1: Connection established.
debug1: identity file /home/ubuntu/.ssh/id_rsa type -1
debug1: identity file /home/ubuntu/.ssh/id_rsa-cert type -1
... some more stuff ...
```

Ok. That's better. IP address is what I was hoping for.
```
ssh -vT git@192.30.253.112
```
Running `ssh -vT` from local terminal went through with no errors.

So I edited `~.ssh/config`, adding:

```
Host github.com
    HostName 192.30.253.112
```

And again:
```
ssh -vT git@github.com
```

No errors. Great!
Git commands `git push` and `git clone` went through ok as well.

Ping me if it helps you too.
