# Git - ***Notes***

## Cloning a repository

Getting a repository from git hub.

```git
git clone <URL>
```

```git
git clone https://github.com/tcdahlberg/cci_test_1.git
```

## Adding a remote repository to an already existing local reporitory

```git
git remote add origin <repository_URL>
git push --set-upstream origin master
```
You should set a personal access token on GIT hub for this that you can easily revoke. [Personal Access Tokens](https://github.com/settings/tokens)

Add your git credentials with the token above into the repository usint these commands (Warning: you will be storing your token in plan text on your computer. This is why you set up that token).

```git
git config credential.helper store
git push
```

```git
 git push --set-upstream origin master
 ```
 
 ## Colaboration

Your git user must be set up as a collaborator in order to commit to any repository. Go to the repository, if you own it, and click on security. Add collaborators here.

Create a new branch:

```git
git checkout -b feature_branch_name
```

Edit, add and commit your files.

Push your branch to the remote repository:

```git
git push -u origin feature_branch_name
```

Listing current branchs:

```git
git branch
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
