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
	4. Now to push the local repo to remote use the command ```git push -u origin master```. Here **origin** refers to the remote repo where we want to push the code and **master** refers to the branch of the local repo we want to push. The __-u or --set-upstream__ here is used to prevent us from specifying the repo and branch again and again. 
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
	4. Run ```git push heroku master``` to push the local branch to heroku, which will deploy this branch. Heroku only deploys the master branch

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
	3. OR you could club the first 2 steps and just do ```git checkout -b <branch name>```. This will first create a branch and then position the HEAD to the current branch.

- How to merge 2 branches (Merge cat branch into master branch)?
	1. Move to the master branch using ```git checkout master```.
	2. Do ```git merge cat```. Please have a look at the image below for better understanding.
	3. Now we can just go and delete the cat branch using ```git branch -d cat```. As you can see in the below image that after deleting the cat branch the commit of the cat branch is moved to the master branch and so there is not extra merge commit in this master branch.This is a fast-forwarding commit.
<img width="623" alt="screen shot 2018-08-25 at 11 35 55 pm" src="https://user-images.githubusercontent.com/12914629/44621218-b1cca980-a8bf-11e8-93b6-1d42792f08a9.png">
<img width="606" alt="screen shot 2018-08-26 at 12 02 57 am" src="https://user-images.githubusercontent.com/12914629/44621399-6ae0b300-a8c3-11e8-8240-5ca1b5c9101c.png">

- What is fast-forwanding?

When we do 1 or multiple commits on 1 branch and nothing on the other branch, it becomes very easy for the git to merge these branches and therefore it fastforwards the merging process.

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
	1. It fetches/sync our local repository with the remote repository. Same as ```git fetch```. It creates a branch named **origin/master** in our local repo. This is the same as the master remote branch.
	2. Then it merge the origin/master with master banch. Same as ```git merge origin\master```.
	3. This will open the VI and show the commit message.Type :wq to save and quit.
	<img width="704" alt="screen shot 2018-08-26 at 1 07 28 am" src="https://user-images.githubusercontent.com/12914629/44621921-8c926800-a8cc-11e8-9f25-14b0aab524db.png">
	<img width="693" alt="screen shot 2018-08-26 at 1 09 09 am" src="https://user-images.githubusercontent.com/12914629/44621930-adf35400-a8cc-11e8-8903-68f207ee8db4.png">

	4. Till now the remote master branch doesn't know about the merge. So do ```git push``` to push your local changes to the remote branch.

- What happens when there is a confict in pull?
	1. During a confict the fetch/sync process executes successfuly and adds the branch origin/master to our local repo.
	2. Issues comes with the merge process. It asks us to make the changes in the conflicted files and then commit. So do ```git status``` to list the files who both have udated and which have a conflict and resolve them.
	3. Then run ```git commit -a```. Note here I have not given any commit message so the default commit message will be shown along with the conflicted files which have been fixed.

<img width="701" alt="screen shot 2018-08-26 at 1 30 41 am" src="https://user-images.githubusercontent.com/12914629/44622136-81d9d200-a8d0-11e8-8ac3-568f3653e0ac.png">
<img width="700" alt="screen shot 2018-08-26 at 1 31 22 am" src="https://user-images.githubusercontent.com/12914629/44622104-e7798e80-a8cf-11e8-816c-e79a91c01412.png">
<img width="670" alt="screen shot 2018-08-26 at 1 32 12 am" src="https://user-images.githubusercontent.com/12914629/44622111-e9435200-a8cf-11e8-9552-e7b56fedcef4.png">


### Branching

- How to create a local branch and push it to remote ?
	1. Create a new branch localy by ```git checkout -b <branchname>```.
	2. ```git push origin <branchname>```. Once you push this branch to the remote, the git starts tracking this branch so that you don't need to provide the branchname again during push. So next time you could simply do ```git push```.

- How to pull this new branch on local ?
	1. When you do a git pull/fetch it will show you there is a new branch.
	2. Although it will not be visible when you do a ```git branch``` since it is still not a tracked branch, but it will be shown in ```git branch -r```.
	3. Now just do a ```git checkout <branchname>``` and it will set this branch as a tracking remote branch. Now you just need to do git push to push any local changes to the remote. 

<img width="604" alt="screen shot 2018-08-29 at 11 16 09 pm" src="https://user-images.githubusercontent.com/12914629/44805377-a7711f00-abe1-11e8-9241-3e1716ab41a8.png">
<img width="687" alt="screen shot 2018-08-29 at 11 15 28 pm" src="https://user-images.githubusercontent.com/12914629/44805380-a8a24c00-abe1-11e8-96d1-1eeabba6ce5a.png">

- How to keep track of the remote?
	1. ```git remote show origin``` : 
		1. Shows all the remote branches and weather they are tracked with local or not.
		2. Local branches configured for git pull.
		3. Local branches configured for git push. It also shows if our local branch is out of date with the remote.

<img width="592" alt="screen shot 2018-08-30 at 12 00 21 am" src="https://user-images.githubusercontent.com/12914629/44807683-ca063680-abe7-11e8-85aa-59df7f30ba71.png">

- How to push a local branch which has been deleted from the remote?
	1. First we try to run ```git push```. This does nothing since there is not remote branch to push to.
	2. Then we run ```git remote show origin```. This will list all the remote braches. The deleted branch appears as a stale branch.
	3. Then run ```git remote prune origin```. This will remove the stale ref of the remote branch from the local.
	4. Then run ```git push origin <branchname>``` to push the branch to remote.
<img width="704" alt="screen shot 2018-08-30 at 12 08 34 am" src="https://user-images.githubusercontent.com/12914629/44808067-e060c200-abe8-11e8-9f32-e81642097202.png">

- What are tags?

Tags are reference to a specific commit. This is generaly used for release versioning. 
1. ```git tag``` lists all the tags in the repo.
2. ```git checkout <tagname>``` will checkout the code for the commit which was tagged.
3. ```git tag -a <tagname> -m "<tagdescription>"``` is used to add a tag with the given name and description.
4. ```git push --tag``` is used to push the tags to remote repo.

<img width="608" alt="screen shot 2018-08-30 at 12 32 23 am" src="https://user-images.githubusercontent.com/12914629/44809398-35520780-abec-11e8-9de0-828656671dd3.png">

### Rebase
This is a mechanism to prevent the merge commits from happening and just merge 2 branches without an additional merge commit.

- Instead of doing ~~git pull~~ we do ```git fetch + git rebase```.
- git rebase does 3 things :
	1. Moves all the commits in master which are not in origin/master to a temporary location.
	2. Run all the origin/master commits one at a time OR moves commits from origin/master to master one at a time.
	3. Run all the commits in temporary area on the master one at a time OR move commits from temporary are to master one at a time.
So there is no merge commit here. Just the commits of the individual branches.
<img width="414" alt="screen shot 2018-08-30 at 11 06 32 pm" src="https://user-images.githubusercontent.com/12914629/44868816-94298680-aca9-11e8-822f-96662442133a.png">
<img width="554" alt="screen shot 2018-08-30 at 11 06 53 pm" src="https://user-images.githubusercontent.com/12914629/44868817-955ab380-aca9-11e8-978c-360a2a72bf84.png">
<img width="324" alt="screen shot 2018-08-30 at 11 07 13 pm" src="https://user-images.githubusercontent.com/12914629/44868822-968be080-aca9-11e8-8d89-59b0526c5ee7.png">
<img width="699" alt="screen shot 2018-08-30 at 11 07 27 pm" src="https://user-images.githubusercontent.com/12914629/44868828-9986d100-aca9-11e8-88e0-38248c986fb9.png">

- How to merge an admin branch into the master branch without a merge commit using rebase?
	1. ```git checkout admin```
	2. ```git rebase master```
		This will push all the commits of master into admin branch before all the new commits in admin branch.
	3. ```git checkout master```
	4. ```git merge admin```. 
		**Note** => This will be a fastforward merge instead of a recursive merge since we have already move all the commits in master to admin using ```git rebase master```.

- How to merge a remote master branch into master branch without a merge commit using rebase?
	1. ```git checkout master```
	2. ```git fetch``` : Syncs the local repo with the remote repo.
	3. ```git rebase``` : This will push all the commits in origin/master to master and add the additional commits in master on top of them.

- What to do in-case of a conflict in rebase?

Suppose you are trying to rebase orgin/master into master and face a conflict. Below are the steps you would follow to resolve it :
1. ```git checkout master```
2. ```git fetch```
3. ```git rebase``` : As soon as you run this command, git will show you a confict in the 2 branches ie master and origin/master. At this point in time if you run a ```git status``` you will find that you are not on any branch but in the middle of a rebase.
	Now you have the following 3 options to chose from :
	1. ```git rebase --continue``` : Applies the commit and continues with the rebase. Before you run this command you must resolve all the conflicts and add them to staging using ```git add <filename>```.
	2. ```git rebase --skip``` : Skips the commit and continues with the rebase.
	3. ```git rebase --abort``` : Aborts/Discards all the commits and restores the branch to the state before rebase.


<img width="897" alt="screen shot 2018-08-30 at 11 17 54 pm" src="https://user-images.githubusercontent.com/12914629/44869816-2763bb80-acac-11e8-929c-ba3838a65bbf.png">
<img width="924" alt="screen shot 2018-08-30 at 11 18 27 pm" src="https://user-images.githubusercontent.com/12914629/44869817-27fc5200-acac-11e8-972f-1485c2507aa8.png">
<img width="998" alt="screen shot 2018-08-30 at 11 20 05 pm" src="https://user-images.githubusercontent.com/12914629/44869819-292d7f00-acac-11e8-8f11-182af0fc3438.png">
<img width="818" alt="screen shot 2018-08-30 at 11 20 38 pm" src="https://user-images.githubusercontent.com/12914629/44869824-2a5eac00-acac-11e8-9546-462a6874d860.png">
<img width="809" alt="screen shot 2018-08-30 at 11 21 14 pm" src="https://user-images.githubusercontent.com/12914629/44869828-2cc10600-acac-11e8-9a4a-52daa7e2bf15.png">
<img width="991" alt="screen shot 2018-08-30 at 11 24 44 pm" src="https://user-images.githubusercontent.com/12914629/44869831-2df23300-acac-11e8-8d79-65c64e6f00c2.png">
<img width="879" alt="screen shot 2018-08-30 at 11 25 36 pm" src="https://user-images.githubusercontent.com/12914629/44869837-2e8ac980-acac-11e8-89ba-2088a9f15808.png">	


### History and Configuration

- What are the general things in a commit details?
	1. SHA Hash
	2. Author
	3. Date
	4. Message

<img width="529" alt="screen shot 2018-08-31 at 7 16 12 pm" src="https://user-images.githubusercontent.com/12914629/44916058-5896c700-ad52-11e8-8ee4-a76776ec6025.png">

- Ways to improve the output of the log ?
	1. ```git log --pretty=oneline``` => SHA + Commit message
	<img width="633" alt="screen shot 2018-08-31 at 7 18 33 pm" src="https://user-images.githubusercontent.com/12914629/44916170-ab707e80-ad52-11e8-8c9a-25397d6699a3.png">

	2. ```git log --pretty=format:"%h %ah- %s [%an]"``` => To provide a custom format for the log
		<img width="630" alt="screen shot 2018-08-31 at 7 20 51 pm" src="https://user-images.githubusercontent.com/12914629/44916291-fd190900-ad52-11e8-8b27-ced406f46885.png">

	3. ```git log --oneline -p``` => This will show you what changed in each commit. Here __-p__ stands for patch.
		<img width="550" alt="screen shot 2018-08-31 at 7 21 38 pm" src="https://user-images.githubusercontent.com/12914629/44916333-1e79f500-ad53-11e8-8b1d-75d9f797f6e4.png">

	4. ```git log --oneline --stack``` => It will show you how many insertions and deletions were made in each commit.
		<img width="630" alt="screen shot 2018-08-31 at 7 24 52 pm" src="https://user-images.githubusercontent.com/12914629/44916512-90523e80-ad53-11e8-8a4b-d87ded7ae9eb.png">

	5. ```git log --oneline --graph``` => This will give us a visual representation of the commits.
		<img width="683" alt="screen shot 2018-08-31 at 7 26 29 pm" src="https://user-images.githubusercontent.com/12914629/44916608-d1e2e980-ad53-11e8-90d8-a13b92d2c279.png">

- How to get the logs for certain range or dates ?
	1. ```git log --until=1.minute.ago``` => Shows commits done until the time specified.We can have multiple values in place of minute like **hour,day,month**.
	2. ```git log --since=1.day.ago``` => Show commits since the time specified. We could also specify both --since and --until in the same statement like ```git log --until=1.minute.ago --since=1.day.ago```.
		<img width="782" alt="screen shot 2018-08-31 at 7 33 54 pm" src="https://user-images.githubusercontent.com/12914629/44917016-d1971e00-ad54-11e8-8009-100f1e3e0b38.png">

- How to find the diff with commits before most resent commit ?
	1. ```git diff HEAD^``` => Show diff parent of latest commit.
		Similarly ```git diff HEAD^^``` will show diff with parent of parent of latest commit.
	2. ```git diff HEAD~5``` => Shows diff with 5 commits ago.
	3. ```git diff HEAD^...HEAD``` => Shows diff of second most recent to the most recent commit.
		<img width="775" alt="screen shot 2018-08-31 at 7 45 58 pm" src="https://user-images.githubusercontent.com/12914629/44917615-80882980-ad56-11e8-8a67-a3ac08249f0b.png">
	4. ```git diff <SHA1> <SHA2>``` => Here **SHA1** and **SHA2** are the commit ID/SHA specifically. So this will show 	the diff between the commits with the given SHA's. You could also use abbreviated SHA provided by the git in 		place of actual SHA's.
	5. ```git diff master admin``` => Shows diff between branch with name "master" and branch with name "admin".
	6. ```git diff --since=1.day.ago``` => This shows time based diffs.
		<img width="794" alt="screen shot 2018-08-31 at 7 55 10 pm" src="https://user-images.githubusercontent.com/12914629/44918107-c98cad80-ad57-11e8-8ae3-1f469fb2ba9c.png">

- How to find the changes in one perticular file?


	You should use ```git blame``` for this.
	1. ```git blame index.html --date sort``` => Shows all the changes made in the file with the author,date,SHA and line 	number which was changed.

		<img width="560" alt="screen shot 2018-08-31 at 8 03 33 pm" src="https://user-images.githubusercontent.com/12914629/44918586-fa211700-ad58-11e8-8d0f-979bb739247a.png">

- What to do if you want to keep a perticular file or folder only to your local repo and don't want to move it to the 	remote repo?


	You should move that file or folder to the __.git/info/exclude__ file so that the git excludes it from being pushed to the remote repo. Suppose us put "experiment/" in this file. Now if you run ```git status``` you wouldn't see this folder anymore.
	
	<img width="723" alt="screen shot 2018-08-31 at 8 14 52 pm" src="https://user-images.githubusercontent.com/12914629/44919203-8f70db00-ad5a-11e8-8991-760156551446.png">

- What to do if you don't want anyone to push a file or folder to git?


	Just add that to the __.gitignore__ file. All the files or folder present in the .gitignore file are not tracked by git.

- How to remove a file/folder from the repository and the file system?


	```git rm <filename OR foldername>``` => Removes the file/folder from the file system and adds that to the stage. Then just do a ```git commit -a -m "msg"``` to push the change to the repo and files should be removed from the repo. Alternatively you could manualy remove the file/folder, then do a ```git add --all``` and then ```git commit -a -m "msg"```.
	<img width="495" alt="screen shot 2018-08-31 at 8 22 54 pm" src="https://user-images.githubusercontent.com/12914629/44919903-330ebb00-ad5c-11e8-8ec3-d5d81b41126d.png">

- How to remove a file/folder from the repository but keep in the file system and prevent git from tracking it going forward?


	```git rm --cached <file OR folder name>``` => Stops tracking the file and adds it to the stage. Then just do a ```git commit -a -m "msg"``` to push the change to the repo and files should be removed from the repo.
	<img width="792" alt="screen shot 2018-08-31 at 8 51 36 pm" src="https://user-images.githubusercontent.com/12914629/44921256-af56cd80-ad5f-11e8-9fe6-4b7ca999042e.png">


- How to define an alias? Like alias for different log formats?

	1. ```git config --global alias.mylog "log --pretty=format:'%h %s [%an]' --graph"``` => 
		Then just do ```git mylog```.
	2. ```git config --global alias.lol "log --graph --decorate --pretty=oneline --abbrev-commit --all"```

<img width="718" alt="screen shot 2018-08-31 at 9 09 59 pm" src="https://user-images.githubusercontent.com/12914629/44922402-f98d7e00-ad62-11e8-9932-4efc5390e274.png">
<img width="773" alt="screen shot 2018-08-31 at 9 14 45 pm" src="https://user-images.githubusercontent.com/12914629/44922403-fa261480-ad62-11e8-8761-e94b3a19c687.png">

### Interactive Rebase



## Cheat-Sheet

### rm

- ```git rm <filename OR foldername>```

 	Removes the file/folder from the file system and adds that to the stage. Then just do a ```git commit -a -m "msg"``` to push the change to the repo and files should be removed from the repo. Alternatively you could manualy remove the file/folder, then do a ```git add --all``` and then ```git commit -a -m "msg"```.

- ```git rm --cached <file OR folder name>``` 

	Stops tracking the file and adds it to the stage. Then just do a ```git commit -a -m "msg"``` to push the change to the repo and files should be removed from the repo.

### blame

```git blame index.html --date sort``` 

	Shows all the changes made in the file with the author,date,SHA and line number which was changed.

### config

- ```git config --global user.email "Ashish@gmail.com"```

	To configure the email globally

- ```git config --global user.name "Ashish"```

	To configure the username globally

- ```git config user.email "Ashish@gmail.com"```

	To configure email for current repo

- ```git config user.email```

	Tells the email being used for this repo

- ```git config --global color.ui true```

	To active the cli color scheme

- ```git config --global core.editor <Editor>``` 

	To configure a core editor like emacs

- ```git config --global merge.tool <merge tool>```

	To confiture a specific merge tool like opendiff

- ```git config --list```

	Shows a list of all the configurations done by you. First shows the global configurations followed by the local configurations.

- ```git config --global alias.mylog "log --pretty=format:'%h %s [%an]' --graph"```

		Then just do ```git mylog```

- ```git config --global alias.lol "log --graph --decorate --pretty=oneline --abbrev-commit --all"```

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

- ```git diff``` OR ```git diff HEAD```
	
	Show all unstaged differences.
- ```git diff --staged```
	
	Show all staged differences.
- ```git diff <filepath>```
	
	This will show the difference in the file at the given filepath in the current working directory from the last commit.
- ```git diff <filepath> --staged```
	
	This will show the difference in the file at the given filepath in the staging area from the last commit.

- ```git diff HEAD^``` 

	Show diff parent of latest commit.Similarly ```git diff HEAD^^``` will show diff with parent of parent of latest commit.

-  ```git diff HEAD~5```
	
	Shows diff with 5 commits ago.

```git diff HEAD^...HEAD```

	Shows diff of second most recent to the most recent commit.

- ```git diff <SHA1> <SHA2>``` 

	Here **SHA1** and **SHA2** are the commit ID/SHA specifically. So this will show 	the diff between the commits with the given SHA's. You could also use abbreviated SHA provided by the git in place of actual SHA's.

- ```git diff master admin``` 
	
	Shows diff between branch with name "master" and branch with name "admin".

- ```git diff --since=1.day.ago``` 

	This shows time based diffs.

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

- ```git log --pretty=oneline``` 

	SHA + Commit message
	
- ```git log --pretty=format:"%h %ah- %s [%an]"``` 

	To provide a custom format for the log
		
- ```git log --oneline -p``` 
	
	This will show you what changed in each commit. Here __-p__ stands for patch.
		
- ```git log --oneline --stack``` 
	
	It will show you how many insertions and deletions were made in each commit.
		
- ```git log --oneline --graph``` 

	This will give us a visual representation of the commits.

- ```git log --until=1.minute.ago``` 

	Shows commits done until the time specified.We can have multiple values in place of minute like **hour,day,month**.

- ```git log --since=1.day.ago``` 

	Show commits since the time specified. We could also specify both --since and --until in the same statement like ```git log --until=1.minute.ago --since=1.day.ago```.
		
### remote

- ```git remote add origin <Remote repository http url>```
	
	This will add a new repository with the name origin and the given git url 
- ```git remote -v```
	
	This will tell you about the remote repository in use
- ```git remote rm <name>```
	
	Removes the remote repository
- ```git remote show origin```

	Show all the remote branches and tells if they are tracked with local repo or not.
- ```git remote prune origin```

	Removes all the brances which have been deleted from the remote but still has a ref in the local repo. These branches are shown as stale branches when you do ```git remote show origin```.

### push

- ```git push -u origin master```

	Pushes the master branch to the remote repository which in this case is origin. Using -u we don't need to provide the repo namewhich is origin in this case and the branch name which is master in this case again and again. So next time we just do 
- ```git push```

	Used to push tracked branches.
- ```git push origin :<branch name>```

	To delete remote branch with the given branch name.
- ```git push origin <local branch name>:<remote branch name>```

	This pushes the local branch with the given name to the remote branch with the given name.

### pull

- ```git pull```

	git fetch + git merge origin/master (If you are on the master branch)
- ```git pull origin <branch name>```

	This pulls the branch with the given name from the remote repo specified by origin

### clone

- ```git clone <Remote repository http url> <folder name>```

	This clones the remote repo specified in the url to local in the specified folder or directory.

### branch

- ```git branch <branch name>```

	Creates a new branch with the given branch name.
- ```git branch -d <branch name>```

	This deletes the branch with the given "branch name". Prevents you from deleting this branch if there are chages which haven't been commited. If you still want to delete it use ```git branch -D <branch name>``` instead.
- ```git branch```

	Lists all the tracked brances in the local repo.
- ```git branch -r```

	Lists all the branches in the remote repo.

### checkout

- ```git checkout <branch name>```

	Swithes the branch/timeline and sets the HEAD to the last commit of the new branch/timeline.
- ```git checkout -b <branch name>```

	Creates a new branch and checks it out for us. Same as ```git branch <branchname> + git checkout <branchname>```.

### merge

- ```git merge <branch name>```

	This merges the branch with the given branch name to the branch which is checked out OR to the branch where the HEAD is pointing.

### stash

- ```git stash```
	
	To push/save all the uncommitted and tracked files onto a stack so that they can be reapplied later.
- ```git stash list```
	
	Shows a list of all the stash you have applied
- ```git stash apply```
	
	Reapplies the latest stash onto the working directory
- ```git stash apply stash@{1}```

	Reapplies the "stash@{1}" stash onto the current working directory

### fetch

- ```git fetch``` 

	Syncs the remote repo with the local repo

### rebase

- ```git rebase```

	Pushes the commits of the remote branch into the local branch followed by the commits of the local branch.

- ```git rebase <branchname>```

	Pushes the commits of the branch with the given branchname to the branch which is checked out followed by the commits in the checked out branch.

- ```git rebase --continue```

	Continues with the rebase after the conflict is resolved.

- ```git rebase --skip```

	Continues with rebase and skips the patch/commit which needs to be applied.

- ```git rebase --abort```

	To checkout the original branch and stop rebase run.