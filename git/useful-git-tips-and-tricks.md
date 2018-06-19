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

## Delete all untracked files

To check what files are going to be delted run:
```
git clean -n
```

To remove files run
```
git clean -f
```

## Change modify commit author

Changing commit author means updating user email and user name for particular
commit or commits. It can be done manually with `git rebase` command or
alternatively you can use create an alias to command automating the change.

### Change commit author with `git rebase -i`

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

**Modify all commit authors**  
If you want to change all commits authors and you are using vim you can use
global replace with regular expression. Hit `[esc]` and type:

```
:g/^pick/s/pick/edit/g
```

### Change commit author with `git rebase -i`
<-- source:
https://github.com/brauliobo/gitconfig/blob/master/configs/.gitconfig
http://stackoverflow.com/a/11768843/6096446
-->

Setup `change-commits` alias

```
# git change-commits GIT_COMMITTER_NAME "old name" "new name"
change-commits = "!f() { VAR=$1; OLD=$2; NEW=$3; shift 3; git filter-branch --env-filter \"if [[ \\\"$`echo $VAR`\\\" = '$OLD' ]]; then export $VAR='$NEW'; fi\" $@; }; f "
```

You can now change author and committer name of all the commits by:
```
git change-commits GIT_AUTHOR_NAME "old name" "new name"
git change-commits GIT_COMMITTER_NAME "old name" "new name"
```

Or you can modify author and committer email of last 10 commits:

```
git change-commits GIT_AUTHOR_EMAIL "old@email.com" "new@email.com" HEAD~10..HEAD
git change-commits GIT_COMMITTER_EMAIL "old@email.com" "new@email.com" HEAD~10..HEAD
```


## Securely remove files from repository
<-- source:
https://github.com/brauliobo/gitconfig/blob/master/configs/.gitconfig
-->

```  
# from https://help.github.com/articles/remove-sensitive-data
remove-file = "!f() { git filter-branch -f --index-filter \"git rm --cached --ignore-unmatch $1\" --prune-empty --tag-name-filter cat -- --all; }; f"
```
