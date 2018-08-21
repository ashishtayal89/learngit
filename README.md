# learngit

## CheatSheet

### config

- ```git config --global user.name "Ashish"```

	To configure the username globally
- ```git config --global color.ui true```

	To active the cli color scheme
git init
	This is to initiate/create a local git repository.
git help
	This tells us about all the commands in git
git help <name>
	This will tell us about the "name" command in detail.
git status
	Tells us about the current status of your working directory
git add <filename> 
	This will add the individual file with the given "filename" to stage.
git add --all,
	This will add all the files to stage.
git add *.txt
	This will add all the files with .txt extension in the current directory to stage.
git add "*.txt"
	This will add all the files with .txt extension in all the directory to stage. 
git add doc/*.txt
	This will add all the files inside the doc directory with the extension .txt to stage.
git add doc/
	This will add all the files inside the doc directory to stage.
git commit -m "Create a Readme"
	This will commit the staged files with the comment "Create a Readme".
git commit -a -m "Created a README"
	This will first add all the tracked files to stage and then commit them with the comment "Create a Readme".
git commit --amend -m "Created a README and added new App file"
	It amends or updates the last commit and changes the previous commit message with the new message specified.
	Should not do this after we have pushed a commit.
git diff <filepath>
	This will show the difference in the file at the given filepath in the current working directory from the last commit.
git diff <filepath> --staged
	This will show the difference in the file at the given filepath in the staging area from the last commit.
git reset HEAD <Filename> 
	This will un-stage the file with the given Filename.
git reset --soft HEAD^ 
	Undo the last commit and adds everything to staging. Should not do this after we have pushed a commit.
git reset --hard HEAD^
	Undo last commit and all the changes are discarded. Should not do this after we have pushed a commit.
git reset --hard HEAD^^
	Undo last 2 commits and all the changes are discarded. Should not do this after we have pushed a commit.
git checkout -- <Filename>
	This will rollback the file to the last commit
git log
	This will list all the commits on the branch along with the author name and date.
git remote add origin "Remote repository http url"
	This will add a new repository with the name origin and the given git url 
git remote -v
	This will tell you about the remote repository in use
git remote rm <name>
	Removes the remote repository
git remote show origin
git remote prune origin
git push -u origin master
	Pushes the master branch to the remote repository which in this case is origin. Using -u we don't need to provide the repo name
	which is origin in this case and the branch name which is master in this case again and again. So next time we just do git push.
git push origin :<branch name> (To delete remote branch)
git push origin <local branch name>:<remote branch name>
git pull
git pull origin <branch name>
git clone <Remote repository http url> <folder name>
git branch <branch name>
git branch -d <branch name>
git branch
git branch -r
git checkout <branch name>
git checkout -b <branch name>
git merger <branch name>
git pull => git fetch + git merge origin/master (If you are on the master branch)
git stash 
	To push/save all the uncommitted and tracked files onto a stack so that they can be reapplied later.
git stash list
	Shows a list of all the stash you have applied
git stash apply
	Reapplies the latest stash onto the working directory
git stash apply stash@{1}
	Reapplies the "stash@{1}" stash onto the current working directory

-Heroku only deploys the master branch
