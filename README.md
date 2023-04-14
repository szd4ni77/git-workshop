# Git workshop

## Topics briefly

- (the gitignore)
- [Configuration](#configuration)
- [Feature branch](#feature-branch)
- [Add](#add)
- [Modification](#modification)
- Stash
- Stage
- Create commit
- Reword/Amend
- Merge with fast-forward
- Merge with merge commit - repo settings for default behaviour
- Rebase
- Interactive rebase, reword, amend, squash, ...
- Handling conflicts (merge, rebase)
- Mass feature branches -> cherry-pick (temp branch or remote)
- Remote tracking branches, push, fetch, pull
- Force push
- Delete remote branch
- 
## Configuration
```shell
git config --list --show-origin
git config --global user.name "Developer Name"
git config --global user.email "developer.name@ip-camp.com"
git config --global alias.st status
git config --local merge.ff only
```

## Feature branch
```shell
git checkout -b feature/thing-to-do
```
For now:
```shell
git checkout -b developer.name/develop
git checkout -b developer.name/feature/thing-to-do
```

## Add
```shell
touch someFile
git status
git add someFile
git status
```

## Modification
```shell
echo "newLine" > fileToModify
git status
```