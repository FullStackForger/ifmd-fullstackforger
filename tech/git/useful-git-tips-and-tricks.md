# Useful GIT tips and tricks

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

## Change commit author with `git rebase -i`

Lets assume your commit history looks like this
```
(HEAD) 7b40993 - 1bce63c - bde7294 - 5729eea
```

If you you want to modify first two commits rebase at first one
```
git rebase -i 5729eea
```

It will open your message file which should look something like this.
```
pick 7b40993 some more changes and fixes (HEAD)
pick 1bce63c docs and examples update
pick bde7294 .gitignore added
pick 5729eea docs created
```

Now just change `pick` to `edit` next to the commits you want to alter.
```
pick 7b40993 some more changes and fixes (HEAD)
pick 1bce63c docs and examples update
edit bde7294 .gitignore added
edit 5729eea docs created
```
Save changes and quit. Rebase will first pause at first commit `5729eea`.
To change the author use 
```
git commit --amend --author="Author Name <email@address.com>"
```
to continue to next edited commit:  
```
rebase --continue
``` 
Continue until rebase is complete.


### Modify all commit authors
If you want to change all commits authors and you are using vim here is nice trick. Hit `[esc]` and type:

```
:g/^pick/s/pick/edit/g
```