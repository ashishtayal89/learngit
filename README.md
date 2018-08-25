# Learn GIT
This document contains some basics of git along with a cheat sheet for git CLI commands.

## Basics

### Introduction

- Created by : **Linus Torvalds** 

- Official site : http://git-scm.com

- Why do we need a version control system :
	1. Merging
	2. Time Capsule OR Maintains a list of different changes/commits at diffreent time in history.

<img width="1368" alt="screen shot 2018-08-22 at 10 57 50 pm" src="https://user-images.githubusercontent.com/12914629/44542058-6d0f0a00-a729-11e8-9916-6602542c53b2.png">

**CVCS** | **DVCS** 
--- | --- 
They have a centralised repository.(Only remote) | The have distributed repository(Remote and local) 
Not easy to make frequent commits | Its easy to make commits since you work on your local repo
Very difficult to work offline | Very easy to work offline
Less secure since the code is only at 1 server | More secure since every individual has a complete copy of the code.
Eg : SVN | Eg : Git

- Flow 
	1. Create a local repo by git init(https://git-scm.com/docs/git-init).
	2. Create a new file Readme.md
	3. Add this file to the staging area using ```git add```. Getting ready to take a snapshot. 
	4. Take a snapshot of the staged changes using ```git commit```.

- Try to keep the commit messages in the present tense. It should explain what the commit does.

### Staging and Remotes

-	Use **reset** command to unstage the files or undo the commits.
-	**HEAD** points/refers to the last commit on the current branch/timeline. 
-	To update the last commit you can do either of the following :
	1. ```git reset --soft HEAD^```, update the file and commit the updates.
	2. ```git commit --amend -m "Created a README and added new App file"```

- Git doen't take care of the access control. We need some softwares to do that :
	1. **Hosted solutions** : Github or Bitbucket
	2. **Self Managed** : Gitosis or Gitorious

- How to push the local code to the remote repo ?
	1. Create a remote repo in git. This will produce a repo url like https://github.com/ashishtayal14/learngit.git
	2. The you need to map this remote url to a local namespace using the command ```git remote add origin https://github.com/ashishtayal14/learngit.git```. Here **origin** is the namespace/alias which we are giving to the remote repo. We can give any name we want(generaly "origin"). Sometimes your could have mutiple remote repositories(different for dev and prod) for the same codebase. In that case you can add multiple remote to the local repository. This means that the remote and local repository has a **many to many relationship** ie a remote repo can be associated with multiple local repo and local repo can be associted with multiple remote repo.
	3. To list all the remote repo saved in your local use the below command. These remotes are like **bookmarks**.
		```git remote -v```
	4. Now to push the local repo to remote use the command ```git push -u origin master```. Here **origin** refers to the remote repo where we want to push the code and **master** refers to the branch of the local repo we want to push. The **u** here is use to prevent us from specifying the repo and branch again and again. 
	5. If your remote repo is in github it will ask for the github username and password. Provide that.
	6. If you don't want to specify the username and password everytime you push please refer the [this](http://help.github.com/articles/set-up-git) link which talks about password caching. 

- How to pull the remote code to local repo ?
	1. Simply run ```git pull```.

- Heroku(Hosting platform)
	1. Create a heroku account and install "heroku gem".
	2. Run ```heroku create```. This command does a few things :
		1. Creates a remote repo and provides a SSH address for it.
		2. Adds the remote repo to the list of repo in the local with the alias heroku.
	3. Run ```git remote -v``` to see the added remote heroku repo
	4. Run ```git push heroku master``` to push the local branch to heroku, which will deploy this branch.

<img width="675" alt="screen shot 2018-08-24 at 12 58 51 am" src="https://user-images.githubusercontent.com/12914629/44547505-f11cbe00-a738-11e8-8ece-6af605472262.png">

### Cloning and Branching 

- How to create a local repo from a remote repo?
	1. Just do a ```git clone https://github.com/ashishtayal14/learngit.git learngit```.
		1. Here we first specify the repo name followed by the local directory name where we want to clone the repo.
		2. This does 3 basic things for us. Namely :
			1. Clones the remote repo
			2. Add the remote repo url to the list of remote url with namespace origin. Check by using ``` git remote -v```
			3. Checks out the initial branch(likely master). Sets the HEAD to the last commit of that branch.
 
- How do I create my own branch ?
	1. First do a ```git branch <branch name>```. This will create a new branch in the repo with whatever name you give to the branch.
	2. Now just run ```git checkout <branch name>```. This will position the HEAD to the "branch name" given by you. This is like changing timeline.
	3. OR you could club the first 2 steps and just do ```git checkout -b <branch name>. This will first create a branch and then position the HEAD to the current branch.

- How to merge 2 branches (Merge cat branch into master branch)?
	1. Move to the master branch using ```git checkout master```.
	2. Do ```git merge cat```. Please have a look at the image below for better understanding.
	3. Now we can just go and delete the cat branch using ```git branch -d cat```. As you can see in the below image that after deleting the cat branch the commit of the cat branch is moved to the master branch and so there is not extra merge commit in this master branch.
<img width="623" alt="screen shot 2018-08-25 at 11 35 55 pm" src="https://user-images.githubusercontent.com/12914629/44621218-b1cca980-a8bf-11e8-93b6-1d42792f08a9.png">
<img width="606" alt="screen shot 2018-08-26 at 12 02 57 am" src="https://user-images.githubusercontent.com/12914629/44621399-6ae0b300-a8c3-11e8-8240-5ca1b5c9101c.png">

- What is fast-forwanding?
When we 1 or multiple commits on 1 branch and nothing on the other branch, it becomes very easy for the git to merge these branches and therefore it fastforwards the merging process.

- Everytime we do a commit our HEAD moves with it and always remains on the last commit until moved manualy.

- What happens when you try to merge 2 branches with 1 or more commits each?(Refer to image below).
	1. Running ```git merge admin``` will open a VI. VI is a screen-oriented text editor originally created for the Unix operating system. The name "vi" is derived from the shortest unambiguous abbreviation for the ex command visual. If you haven’t defined a core.editor for Git to use, it defaults to using vi for commit messages.
	2. Please enter the vi command and press enter to exit. Refer to the image below for different vi commands. If you simply want to save and exit type __:wq__ and hit enter. Here w => write and q=> quit.
	3. This kind of merge is known as **recursive merge**. In this kind of merge, git creates a commit at the time of merge. This commit has no modified files but just signifies the merge of 2 branches. This is in addition to the commits in both the branches. Hence in a recursive merge there will always be an extra commit in the master branch which is the merge commit.

 
<img width="704" alt="screen shot 2018-08-25 at 11 46 45 pm" src="https://user-images.githubusercontent.com/12914629/44621288-34099d80-a8c1-11e8-8fa9-23432e0cda40.png">
<img width="491" alt="screen shot 2018-08-25 at 11 56 04 pm" src="https://user-images.githubusercontent.com/12914629/44621351-77184080-a8c2-11e8-93f6-a5b0d386020a.png">
<img width="702" alt="screen shot 2018-08-26 at 12 23 02 am" src="https://user-images.githubusercontent.com/12914629/44621587-42a68380-a8c6-11e8-80b2-a1d8e33d6cfb.png">


### Collaboration

- What happens behind the sceens in pull?
	1. It fetches/sync our local repository with the remote repository. Same as ```git fetch```. It creates a branch names **origin/master** in our local repo. This is the same as the master remote branch.
	2. Then it merge the origin/master with master banch. Same as ```git merge origin\master```.
	3. This will open the VI and show the commit message.Type :wq to save and quit.
	<img width="704" alt="screen shot 2018-08-26 at 1 07 28 am" src="https://user-images.githubusercontent.com/12914629/44621921-8c926800-a8cc-11e8-9f25-14b0aab524db.png">
	<img width="693" alt="screen shot 2018-08-26 at 1 09 09 am" src="https://user-images.githubusercontent.com/12914629/44621930-adf35400-a8cc-11e8-8903-68f207ee8db4.png">
	4. Till now the remote master branch doesn't know about the merge. So do ```git push``` to push your local changes to the remote branch.

## Cheat-Sheet

### config

- ```git config --global user.email "Ashish@gmail.com"```

	To configure the email  globally

- ```git config --global user.name "Ashish"```

	To configure the username globally
- ```git config --global color.ui true```

	To active the cli color scheme
### init

- ```git init```

	This is to initiate/create a local git repository. 
	It is created inside the local git directory which is at Users/<name>/store/.git
	
### help 

- ```git help```

	List's all the commands in git
- ```git help <command name>```

	This will tell us about the  command in detail.

### status

- ```git status```

	This tells about the current state/status of your working directory. Majorly
	1. Tracked Files (Staged and Unstaged both).
	2. Untracted Files.

### add
Tells us about the current status of your working directory
	
- ```git add <filename>```

	This will add the individual file with the given "filename" to stage.
- ```git add --all```

	This will add all the files to stage.
- ```git add *.txt```

	This will add all the files with .txt extension in the current directory to stage.
- ```git add "*.txt"```

	This will add all the files with .txt extension in all the directory to stage. 
- ```git add doc/*.txt```

	This will add all the files inside the doc directory with the extension .txt to stage.
- ```git add doc/```

	This will add all the files inside the doc directory	

### commit
This will take a screenshot of all the staged/added changes.

- ```git commit -m "Create a Readme"```

	This will commit the staged files with the comment "Create a Readme".
- ```git commit -a -m "Created a README"```

	This will first add all the tracked files to stage and then commit them with the comment "Create a Readme".
- ```git commit --amend -m "Created a README and added new App file"```

	It amends or updates the last commit and changes the previous commit message with the new message specified.
	Should not do this after we have pushed a commit.

### diff

- ```git diff```
	
	Show all unstaged differences.
- ```git diff --staged```
	
	Show all staged differneces.
- ```git diff <filepath>```
	
	This will show the difference in the file at the given filepath in the current working directory from the last commit.
- ```git diff <filepath> --staged```
	
	This will show the difference in the file at the given filepath in the staging area from the last commit.

### reset

- ```git reset HEAD <Filename>```
	
	This will un-stage the file with the given Filename.
- ```git reset --soft HEAD^```
	
	Undo the last commit and adds everything to staging. Should not do this after we have pushed a commit.
- ```git reset --hard HEAD^```
	
	Undo last commit and all the changes are discarded. Should not do this after we have pushed a commit.
- ```git reset --hard HEAD^^```
	
	Undo last 2 commits and all the changes are discarded. Should not do this after we have pushed a commit.

### checkout

- ```git checkout -- <Filename>```
	
	This will rollback the file to the last commit state. 

### log

- ```git log```
	
	This will list all the commits on the branch along with the author name and date.

### remote

- ```git remote add origin <Remote repository http url>```
	
	This will add a new repository with the name origin and the given git url 
- ```git remote -v```
	
	This will tell you about the remote repository in use
- ```git remote rm <name>```
	
	Removes the remote repository
- ```git remote show origin```
- ```git remote prune origin```

### push

- ```git push -u origin master```

	Pushes the master branch to the remote repository which in this case is origin. Using -u we don't need to provide the repo namewhich is origin in this case and the branch name which is master in this case again and again. So next time we just do 

- ```git push```
- ```git push origin :<branch name> (To delete remote branch)```
- ```git push origin <local branch name>:<remote branch name>```

### pull

- ```git pull```

	git fetch + git merge origin/master (If you are on the master branch)
- ```git pull origin <branch name>```

### clone

- ```git clone <Remote repository http url> <folder name>```

### branch

- ```git branch <branch name>```
- ```git branch -d <branch name>```
	This deletes the branch with the given "branch name".
- ```git branch```
- ```git branch -r```

### checkout

- ```git checkout <branch name>```
- ```git checkout -b <branch name>```

### merge

- ```git merge <branch name>```

### stash

- ```git stash```
	
	To push/save all the uncommitted and tracked files onto a stack so that they can be reapplied later.
- ```git stash list```
	
	Shows a list of all the stash you have applied
- ```git stash apply```
	
	Reapplies the latest stash onto the working directory
- ```git stash apply stash@{1}```

	Reapplies the "stash@{1}" stash onto the current working directory

-Heroku only deploys the master branch
