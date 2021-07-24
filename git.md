# Git global setup
```
git config --global user.name "John Doe"
git config --global user.email "john@mail.com" 
```

# Create a new repository
```
git clone repository remote
cd folder
touch README.md
git add README.md
git commit -m "add README"
git push -u origin master
```

# Push an existing folder
```
cd existing_folder
git init
git remote add origin repository remote
git add .
git commit -m "Initial commit"
git push -u origin master 
```

# Push an existing Git repository
```
cd existing_repo
git remote rename origin old-origin
git remote add origin repository remote
git push -u origin --all
git push -u origin --tags 
```

# Git line change name directory
git clone repository signin

# How to change remote git repository
## 1. List your existing remotes
```
$ git remote -v
```
If you copied the link to the repository from Clone with SSH in your GitLab, the output should look something like this:
```
origin git@your.git.repo.example.com:user/repository.git (fetch)
origin git@your.git.repo.example.com:user/repository.git (push)
```
If you copied the link to the repository from Clone with HTTPS in your GitLab, the output should look something like this:

```
origin https://your.git.repo.example.com/user/repository.git (fetch)
origin https://your.git.repo.example.com/user/repository.git (push)
```

Note: To find the SSH and HTTPS URLs, go to your GitLab, select your project, and click on Clone.

## 2. Change a remote Git repository
We can change the remote repository by using git remote set-url command:
```
$ git remote set-url origin git@your.git.repo.example.com:user/repository2.git
```
The command takes two arguments: existing name of the remote (in our case origin) and a new remote URL (in our case git@your.git.repo.example.com:user/repository2.git)

In case you change your remote repository to https URL, you will be prompted for your username and password next time you use git fetch, git pull or git push.

If you try to use a link to a non-existing remote, you will get the following error:

```
> fatal: No such remote 'origin'
```
When this happens, recheck your URL and make sure it matches the one in your GitLab or GitHub.

If you'd like to learn about Git make sure to check the following articles:

- Push identical code into two git remote repositories
- How to git revert a commit