# Memo GIT

## Ignore files that have already been committed to a Git repository
First commit any changes in repo

```
git rm -r --cached .
git add .
git commit -m ".gitignore is now working"
```
## Discard all local commits
```
git reset --hard origin/master
```





