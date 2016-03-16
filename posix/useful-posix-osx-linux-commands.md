# Useful POSIX (OSX, Linux) commands

## Serching

### Find text in file
Search for string recursively
<!-- todo: more at http://stackoverflow.com/questions/16956810/finding-all-files-containing-a-text-string-on-linux -->

```
grep -R "test to find" /location/directory/
```
### Search current directory for files starting with index
```
find -name index*
```

### Searches user home directory
```
find ~/ -name index*
```

### Search all directories
```
find / -name index
```

### Find files and archive them

```
find /path/to/files -name "*.ext" -type f -exec tar -czf archive.tar.z {} \;
```

### Find files and archive them individually
```
find /path/to/files -name "*.ext" -type f -exec tar -czf {}.tar.gz {} \;
```


### Find files, archive first and then delete them
```
find /path/to/files -name "*.ext" -type f -exec tar -czf archive.tar.z {} \; -exec rm {} \;
```

## Archiving

### Create tar archive from files foo and bar
```
tar -cf archive.tar foo bar
```

### Create tar.gz archive from path dir
```
tar -czvf archive.tar.gz ./path
```

### List all files in archive.tar verbosely
```
tar -tvf archive.tar
```

# Extract all files from archive.tar
```
tar -xf archive.tar
```



## Privileges

### Recursive changing privileges

Give read & execute privileges to all directories:
```
find /path/to/base/dir -type d -exec chmod 755 {} +
```

Give read privileges to all files
```
find /path/to/base/dir -type f -exec chmod 644 {} +
```

## Displaying system information

### System information (processor, kernel, system)
```
uname -a
```

### Display linux distribution
```
cat /etc/issue
```

### Linux distro/distribution and system version

```
cat /etc/*-release
```
**Note:** this one doesn't work on OSX.

### Display file system
```
df -Th
```

### Disk usage files
Display disk usage of files file, recursively in directories in human readable format, on same main file system and summarised on depth 1 from root directory
```
du -h -x --max-depth=1 /
```

## Monitoring

### System processes monitor
```
top
```

### Display uptime
```
uptime
```
### logged in users along with their activities
```
w
```

### Display open ports
```
sudo netstat -anltp
```

```
lsof -i -P | grep -i "listen"
```
more [lsof](http://www.thegeekstuff.com/2012/08/lsof-command-examples/) examples


## Users and groups

### Add user to a group
Add existing `ec2-user` user to an existing `www-data` group
```
usermod -a -G www-data ec2-user
```
