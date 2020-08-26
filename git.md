# GIT Hints, Tips and Snipits

[Home](index.md)

## Quick links

- [Pro Git 2nd Editon](https://git-scm.com/book/en/v2/)
- [TFS Git Command Reference](https://docs.microsoft.com/en-us/vsts/repos/git/command-prompt?view=vsts)

## Basics

### View History One Line Per Commit

```
git log --pretty=oneline
```

```
git log --pretty=format:"%<(9)%h %<(13)%cn %<(12)%cd %s" --date=short
```

## Branches

### List All Branches

```
git branch -a
```

### Create a Local Branch With a Different Name

```
git branch local-name remotes/origin/remote-name
```

### Push a Remote Branch With a Different Name

```
git push origin local-name:remote-name
```

### Create an Orphan Banch

```
git checkout --orphan newbranch
git rm -rf .
```

### Delete a Local Branch

```
git branch -d local-name
```

### Delete a Remote Branch

```
git push origin :remote-name
```

### Squash Merge

```
git merge --squash bugfix
```

## Remotes

### List All Remotes

```
git remote -v
```

### Remove a Remote

```
git remote rm origin
```

## Submodules

### Clone Submodules

```
git submodule update --init --recursive
```

### History

Remove a directory from history:

```
git filter-branch -f --tree-filter "rm -rf directory" --prune-empty HEAD
```
