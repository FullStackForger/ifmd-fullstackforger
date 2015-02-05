# Useful GIT scripts

## Delete all GIT tags from local and remotes

Create file
```
cd /usr/bin/
sudo vim git-delete-all-tags 
```

Paste script
```
#!/bin/sh
for t in `git tag`
do
    git push origin :$t
    git tag -d $t
done
```

Set as executable
```
sudo chmod 755 git-delete-all-tags
```