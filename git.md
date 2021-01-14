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