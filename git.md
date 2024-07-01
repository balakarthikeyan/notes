## Configuring Git:
```
git config --global user.name "balakarthikeyan"
git config --global user.email "balakarthikeyan07@unimoni.com"
git config --global color.ui true
git config --list
```
## Starting a New Local Repository with Git:
```
git init
git status
```
## Staging Files with Git:
```
git add <filename>
git status
git add .  //Current directory
git add --all  //Files and sub folders. Also use the option "-A" instead of "--all"
git rm --cached <filename> //The "--cached" option indicates files in the staging area
git status
git reset <filename> //An alternative to "rm --cached <filename>"
```
##  Committing Changes to Git:
```
git commit -m "Comments"
git commit -a -m "Append Comments" //Add modified files to the staging area and commit
git reset --soft HEAD //Undo the commit
git add <filename>
git commit --amend -m "Add the remaining file" //The "--amend" option lets you amend the last commit by adding a new file
```
## Push and Pull To and From a Remote Repository:
```
git remote add origin http://github.com/balakarthikeyan/notes.git
git push -u origin <branch>
git remote -v
git clone http://github.com/balakarthikeyan/notes.git <branch>
git pull
```
## Working with Branches:
```
git branch
git branch <branch>
git checkout <branch>
git merge <branch>
git branch -d <branch>
git checkout -b <branch> //Instead of running two commands you can run only one
```
## Merge Conflicts

### Fetching remote branch to the 'tmp' local branch 
`git fetch origin <branch>:tmp`

### Rebasing on local 'tmp' branch
`git rebase tmp`

### pushing local changes to the remote
`git push origin HEAD:<branch>`

### Removing temporary created 'tmp' branch
`git branch -D tmp`

## Check Difference
`git diff –color-words`

## Check Log
`git log --oneline`

## Reset Revert Rebase Commands
```
git reset sha-value
git revert HEAD
git checkout feature
git rebase master
```

## gitignore File
```
.gitignore
.DS_Store
Thumbs.db
npm-debug.log
/bower_components
/node_modules
/vendor
```
## Reset Revert Rebase Commands
```
git reset --hard origin/master 
git pull --rebase origin preview
git push --force origin <branch-name>
```
## Commit git with the previous date?
```
git commit --amend --date="YYYY-MM-DD HH:MM:SS"
git commit --date="YYYY-MM-DD HH:MM:SS" -m "Your commit message"
```