# Github & Git
* `github` is a service which provide repository or folder for your code.
* `git` is a software configuration or source code management tool or to manage the different versions of your code.

Source code management tools are of two types:
1. Centrilised version control System (CVCS)
2. Distributed version control System (DVCS)

Three parts of git folder:
1. `Working directory` : it is a part where all your code is present
2. `Staging area` : it is that part where you can add file or code from working directory that is need to be commited or add to local repository
3. `Local repository` : here you can add your final code which you can upload on central server or github

## Configuring Git:
```bash
git config --global user.name "balakarthikeyan"
git config --global user.email "balakarthikeyan07@unimoni.com"
git config --global color.ui true
git config --list
```
## Starting a New Local Repository with Git:
```bash
git init
git status
```
## Staging Files with Git:
```bash
git add <filename>
git status
git add .  //Current directory
git add --all  //Files and sub folders. Also use the option "-A" instead of "--all"
git rm --cached <filename> //The "--cached" option indicates files in the staging area
git status
git reset <filename> //An alternative to "rm --cached <filename>"
```
##  Committing Changes to Git:
```bash
git commit -m "Comments"
git commit -a -m "Append Comments" //Add modified files to the staging area and commit
git reset --soft HEAD //Undo the commit
git add <filename>
git commit --amend -m "Add the remaining file" //The "--amend" option lets you amend the last commit by adding a new file
```
## Push and Pull To and From a Remote Repository:
```bash
git remote add origin http://github.com/balakarthikeyan/notes.git
git push -u origin <branch>
git remote -v
git clone http://github.com/balakarthikeyan/notes.git <branch>
git pull
```
## Working with Branches:
```bash
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
```bash
git reset sha-value
git revert HEAD
git checkout feature
git rebase master
```

## gitignore File
```bash
.gitignore
.DS_Store
Thumbs.db
npm-debug.log
/bower_components
/node_modules
/vendor
```

## Reset, Revert and Rebase Commands
```bash
git revert <commmit_id>
git reset --hard origin/master 
git pull --rebase origin preview
git push --force origin <branch-name>
```

## Commit git with the previous date?
```bash
git commit --amend --date="YYYY-MM-DD HH:MM:SS"
git commit --date="YYYY-MM-DD HH:MM:SS" -m "Your commit message"
```

## Change CLRF
`git config core.autocrlf false`

## Remove local commit
```bash
git rm --cached -r .
git reset --hard
```

## Change remote URL
```bash
git remote set-url origin git_url
git remote set-url origin https://username:your-token@github.com/username/repository.git
```

## How to remove Untracked files
```bash
git clean -n - Dry run to see which files will be deleted
git clean -f - Actually remove the untracked files (use with caution)
```
## Tags
```bash
git tag -a <tagname> -m <messgae> <commit_id>
git tag → to see the list of tag
git show <tag name> - to see a particular commit content by using tag
git tag -d <tagname> - to delete a tag
```

> `git fetch`

It will fetches remote updates (refs and objects) but your local stays the same (i.e. origin/master gets updated but master stays the same) and any new branches to your local Repository. git fetch is similar to pull but doesn't merge.

> `git pull`

It will apply the changes from remote to the current branch in local and instantly merges.

> `git clone`

git clone clones a repo.

> `git rebase`

git rebase will pull down the remote changes, rewind your local branch, replay your changes over the top of your current branch one by one until you're up-to-date.