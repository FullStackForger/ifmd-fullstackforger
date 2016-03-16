# Basic commands


## Workflow

### Push an existing repository from the command line (GitHub example)
```
git push -u origin master
```

## Branches

### list local branches
```
git branch
```

### list remote branches
```
git branch -r
```

### list both local and remote branches
```
git branch -a
```

### switches to branch-name
```
git checkout branch-name
```

### creates new branch from old-branch and switches to it
```
git checkout -b new-branch old-branch
```

### creates new-branch from current branch
```
git branch new-branch
```

### removes branch name (if it is not current branch)
```
git branch -d branch-name
```

## Remotes

### Add remote location to your repository
```
git remote add origin https://github.com/user-name/repository.git
```

### Remove remote location to your repository
```
git remote remove origin
```
