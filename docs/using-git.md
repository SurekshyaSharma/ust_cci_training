# Working with Git and GitHub

[Git Documentation](https://www.git-scm.com/docs)

[GitHub Documentation](https://www.git-scm.com/docs)

## Create a repository

Initializing your git repository can happen from two directions. You can create your repository on your remote first (GitHub) and then clone it down. Alternatively, you can create your local repository first and then connect up your remote later.

### Create local repository

1. Create a project directory and then navigate into it

2. Init project as a git repository

   ```git
   git init
   ```

### Create and Clone a GitHub repository

1. Log in to your GitHub account.

2. Click on the green "New" repository button from left menu

3. Name your repository, give it a description, and decide if it is public or private

4. After the creation of the repository click on the "Clone or download" dropdown menu and copy the URL provided.

5. Navigate to the directory you normally keep your projects. The cloning process will create its own directory. Enter the following command with the URL you copied subsituted for <COPIED_URL>:

   ```git
   git clone <COPIED_URL>
   ```

## Adding a remote repository to an already existing local reporitory

```git
git remote add origin <repository_URL>
git push --set-upstream origin master 
```

## Adding git credentials

You should set a personal access token on GIT hub for this that you can easily revoke. [Personal Access Tokens](https://github.com/settings/tokens)

Add your git credentials with the token above into the repository usint these commands. Warning: you will be storing your token in plan text on your computer. This is why you set up that token. It should also be noted that you may want to set up a secure certificate connection which is a bit more complicated but does not store your token.

```git
git config credential.helper store
git push
```

```git
 git push --set-upstream origin master
 ```
 
 ## Colaboration

Your git user must be set up as a collaborator in order to commit to any repository. Go to the repository, if you own it, and click on security. Add collaborators here.

Create a new branch (and switch to it):

```git
git checkout -b feature_branch_name
```

Edit, add and commit your files.

Push your branch to the remote repository:

```git
git push -u origin feature_branch_name
```

Listing current branches (local and remote):

```git
git branch --a
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
