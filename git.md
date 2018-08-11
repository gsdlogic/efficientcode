# GIT Hints, Tips and Snipits

[Home](index.md)

## Basics

### View History One Line Per Commit

[Documentation](https://git-scm.com/book/en/v2/Git-Basics-Viewing-the-Commit-History)

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
