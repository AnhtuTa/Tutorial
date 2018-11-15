# Demo some commands in git
## These example are useful in projects that have multiple teams participate, and have multiple branch, pull request...

#### First, open new folder, and clone from the repo you want
```
$ git clone repo_url.git
```
#### Check current branch, it will show you the master branch, which means you're in "master" branch
```
$ git branch
```
#### Assume you only have permission to work on "dev" branch, so you have to switch to it
```
$ git checkout dev
```
#### Get newest changes from server in this dev branch
```
$ git pull origin dev
```
#### Create your own new branch to work on it (if your haven't created yet, example: your_branch)
```
$ git checkout -b your_branch
```
#### Now you change some files in your repository, after that, you want to commit these files to your branch, first, check some changes
```
$ git status
```
#### Add all changed files
```
$ git add .
```
#### Assume that you don't want to commit some exceptional files, you can remove these files from commiting (file1 = path to file1):
```
$ git reset -- file1 file2 file3 ...
```
Or if you haven't add changed files yet, and you want to undo a changed file on your local, use:
```
$ git checkout -- filename
```
#### Commit these changed files: If this is the first commit, then use the following command
```
$ git commit -m "your comment here"
```
#### If there's already one/some commit(s) previously, and you want to merge this commit to the previous commit, use the following command
```
$ git commit --amend
```
#### Now you need to push your changes to server. Remember, you're working on "dev" branch, and create new branch in order to create new pull request to merge this new branch to "dev" branch, so you have to pull all the newest changes from server to "dev" branch first, and the use "rebase" to merge all these changes from "dev" to "your_branch"
```
$ git checkout dev  
$ git pull origin dev  
$ git checkout your_branch  
$ git rebase dev  
```
- Note: when you rebase, if you encounter the error: "Cannot rebase: You have unstaged changes. Please commit or stash them.". Which means your branch has some changed files that haven't been committed. You have to commit all these changes before rebasing from dev
- After rebase from dev, some conflicts may exist. You have to resolve all of them to continue to the next step
#### Now push you changes to server (optional: use the -f option to force update to merge these changes to your previous commit on server)
```
$ git push origin your_branch -f
```
## Done!
