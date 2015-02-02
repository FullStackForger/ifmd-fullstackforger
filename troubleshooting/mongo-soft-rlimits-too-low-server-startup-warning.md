# WARNING: soft rlimits too low
 
## Mongo startup warnings

If you have just installed / started MongoDB for the first time, you might get a warning similar to the one below. 

``` 
Server has startup warnings:
2015-02-02T10:41:46.200+0000 [initandlisten]
2015-02-02T10:41:46.200+0000 [initandlisten] ** WARNING: soft rlimits too low.
```
## What are soft rlimits?

In a nutshell, the soft limits are value enforced by the kernel for the corresponding resource. The hard limit is a top end of range for the soft limit. It means that an unprivileged process may only set its soft limit to a value from the threshold from 0 up to the hard limit, and (irreversibly) lower its hard limit. A privileged process is on the other hand allowed to make arbitrary changes to either limit value.


## How to fix "soft rlimits too low" warning

### Temporary fix

If you just want to fix problem temporarily increase soft limits by running:
```
ulimit -n 1024
```

### Persisting the fix after the reboot (on yosemite)

Default ulimit is derived from `launchctl limit` and even though user may change ulimit numbers, they can't exceed launchctl limit hard limits.

That is why it is good idea to create startup plist config.

Just wget [this gist](https://gist.github.com/rusticode/365f3489b5ed51574363) with command below or manualy create file '/Library/LaunchDaemons/my.startup.environment.plist' copying gist content.
```
sudo wget -O /Library/LaunchDaemons/my.startup.environment.plist  https://gist.github.com/rusticode/365f3489b5ed51574363/raw
```

Now use `launchctl` to load mongo jobs.
```
sudo launchctl load -w /Library/LaunchDaemons/my.startup.environment.plist
```

## Resources

- [ulimit command](http://ss64.com/osx/ulimit.html)
- [mongod mac osx rlimits warning](http://stackoverflow.com/questions/16621763/mongod-2mac-os-x-rlimits-warning)
- [persisting ulimits on mac](https://coderwall.com/p/lfjoaq/persist-ulimit-settings-in-mac-os-x)
- [relationship between launchctl and ulimit](http://apple.stackexchange.com/questions/116273/what-is-the-relationship-between-launchctl-limit-and-ulimit)
- [no launchd conf on yosemity](http://stackoverflow.com/questions/25385934/setting-environment-variables-via-launchd-conf-no-longer-works-in-os-x-yosemite)
