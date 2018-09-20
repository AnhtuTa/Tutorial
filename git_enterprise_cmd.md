# Demo some commands in git
## These example are useful in projects that have multiple teams participate, and have multiple branch, pull request...

#### First, open new folder, and clone from the repo you want
$ git clone repo_url.git

#### Check current branch, it will show you the master branch, which means you're in "master" branch
$ git branch

#### Assume you only have permission to work on "dev" branch, so you have to switch to it
$ git checkout dev

#### Get newest changes from server in this dev branch
$ git pull origin dev

#### Create new branch (your own branch, example: your_branch)
$ git checkout -b your_branch

#### Now you change some file in your repository, after that, you want to commit these file to your branch, first, check some changes
$ git status

#### Add all changed file
$ git add .

#### Commit these changed files: If this is the first commit, then use the following command
$ git commit -m "your comment here"

#### If there's already one/some commit(s) previously, and you want to merge this commit to the previous commit, use the following command
$ git commit --amend

#### Now you need to push your changes to server. Remember, you're working on dev branch, and create new branch in order to create new 
pull request to merge this new branch to dev branch, so you have to pull all the newest changes from server from to branch first,
and the use "rebase" to merge all these changes from dev to your_branch
$ git checkout dev
$ git pull origin dev
$ git checkout your_branch
$ git rebase dev

#### Now push you changes to server (use the -f option to force update to merge these changes to your previous commit on server ???)
$ git push origin crud_service_client -f

## Done!
