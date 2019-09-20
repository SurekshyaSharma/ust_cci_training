# Git - ***Notes***

Getting a repository from git hub

```git
git fetch <URL>
```

```git
git fetch https://github.com/tcdahlberg/cci_test_1.git
```

## Git commands I keep using

Resetting my local repository to what is on the remote:

```git
git fetch origin
git reset --hard origin/master
git clean -f
```

Getting new .gitignore entries to work ***Make sure you have committed all other changes before doing this***:

```git
git rm -rf --cached .
git add .
git commit -m "I just updated ignore with..."
```
