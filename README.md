# Git workshop

## Topics briefly

- (the gitignore)
- [Configuration](#configuration)
- [Feature branch](#feature-branch)
- Remote tracking branches, push, fetch, pull
- Force push
- Delete remote branch
- [Add](#add)
- [Modification](#modification)
- [Stash](#stash)
- [Stage](#stage)
- [Commit](#commit)
- [Reword/Amend](#rewordamend)
- [Merge with fast-forward](#merge-with-fast-forward)
- [Merge with merge commit](#merge-with-merge-commit)
- [Rebase](#rebase)
- [Interactive rebase](#interactive-rebase)
- [Conflicts](#conflicts)
- [Mass feature branches](#mass-feature-branches)

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
git restore fileToModify
echo "betterNewLine" > fileToModify
```

## Stash
```shell
git stash # push
git stash show
git stash list
git stash pop
git stash drop
```

## Stage
```shell
git add .
git restore --staged fileToModify
git status
```

## Commit
```shell
git commit # -m "Add someFile"
git log # --graph --all
git add fileToModify
echo "oneMoreLine" >> fileToModify
git commit -a -m "Add new lines"
git log
```

## Reword/Amend
```shell
git commit --amend
```

## Merge with fast-forward
```shell
git checkout developer.name/develop
git merge developer.name/feature/thing-to-do
git branch -d developer.name/feature/thing-to-do
```

## Merge with merge commit
```shell
git checkout -b developer.name/feature/other-thig-to-do

git checkout developer.name/develop
echo "Urgent fix" >> fileToModify
git commit -a -m "Fix this"
git checkout -

echo "New feature" >> someFile
git commit -a -m "Add new feature"

git checkout developer.name/develop
git merge developer.name/feature/other-thing-to-do
git merge --ff developer.name/feature/other-thing-to-do
git branch -d developer.name/feature/other-thing-to-do
```
You can set repo level or global config to allow only fast-forward merge.
You can also override it.

## Rebase
```shell
git checkout -b developer.name/feature/third-thing-to-do

git checkout developer.name/develop
echo "Other bugfix" >> fileToModify
git commit -a -m "Fix other thing"
git checkout -

echo "Other new feature" >> someFile
git commit -a -m "Add other new feature"
# save the branch what we rebase for further use
git checkout -b temp
git checkout -
git rebase developer.name/develop
```

## Interactive rebase
```shell
# go back to saved branch
git reset --hard temp 
echo "Feature 3" >> someFile
git commit -a -m "Add feature 3"
echo "Refactor" >> someFile
git commit -a -m "Refactors related to feature 3"
git rebase -i developer.name/develop 
# pick first, edit second, squas third
# git rebase --abort
echo "Fix in feature 3" >> fileToModify
git commit -a --amend
git rebase --continue
```

Just to finish this:
```shell
git branch -D temp
git checkout developer.name/develop
git merge developer.name/feature/third-thing-to-do
git branch -d developer.name/feature/third-thing-to-do
```

## Conflicts
```shell
git checkout -b developer.name/feature/conflicting
# Edit a line in fileToModify
git commit -a -m "Add modification"

git checkout -
# Edit the same line in fileToModify
git commit -a -m "Add other modification"
git checkout -

git rebase developer.name/develop
# Resolve the conflict
git rebase --continue
```

Just to finish this:
```shell
git checkout developer.name/develop
git merge developer.name/feature/conflicting
git branch -d developer.name/feature/conflicting
```

## Mass feature branches
(don't do this (you will))
```shell
git checkout -b developer.name/feature/first
echo "First" >> fileToModify
git commit -a -m "First"
```
You create a MR from this.
Then continue with new feature built on the previous.
```shell
git checkout -b developer.name/feature/second
echo "Second" >> fileToModify
git commit -a -m "Second"
```
New commits appear on develop.
```shell
git checkout developer.name/develop
echo "Till then" >> fileToModify
git commit -a -m "Till then"
```
You got some feedback and need to change the first branch, rebase on develop and merge.
```shell
git checkout developer.name/feature/first
# Modify first
git commit -a -m "Modification on feedback"
git rebase developer.name/develop
git checkout developer.name/develop
git merge developer.name/feature/first
git branch -d developer.name/feature/first
```
Now try to rebase the second.
```shell
git checkout developer.name/feature/second
git rebase developer.name/develop
```
A commit conflicts with the "same" one.
Solution:
```shell
git rebase --abort
git checkout -b temp
git checkout -
git reset --hard developer.name/develop
git cherry-pick # copy ref here
git git branch -D temp
```