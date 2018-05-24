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

## Clean local repo
```
git reset --hard
git clean -fdx
```


## Credential cache
```
git config --global credential.helper wincred
```

### Move a repo from another repo
```
git clone --mirror http://docker.sipol.dev:8081/CoE-GIS/coegis-lizmap.git
cd coegis-lizmap
git push --mirror https://coegis.pl.s2-eu.capgemini.com/gerrit/p/coegis-lizmap.git
```




