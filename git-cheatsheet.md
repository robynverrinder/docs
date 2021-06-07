# Git cheatsheet

A very bare-bones sample git workflow for the command line - for those moments when you just can't remember that one thing!

## Table of Contents

- [Creating a new repo](#create-repo)  
- [Committing changes](#commit)
- [Making branches](#branch)
- [Merging branches](#merge)
- [Deleting branches](#delete)

---

<a name="create-repo"/>

## Creating a new repo
1. Create the repo on Github
2. Copy the https clone URL under the code download dropdown:
<img src="https://user-images.githubusercontent.com/7614888/120982282-d0eb0c00-c778-11eb-96c7-0e211b901272.png" width=250/>

3. Navigate to the directory where you want the repo to live on your machine
4. Clone the repo using ```git clone paste_clone_url_here```
5. Navigate into the newly created repo directory with ```cd reponame```
6. Set up the [.gitignore](https://github.com/African-Robotics-Unit/docs/blob/main/newcomers-guide.md#the-readmemd-file) file using ```nano .gitignore``` on Mac or Linux, and ```notepad .gitignore``` on Windows
7. Commit and push your changes to establish the connection between your local copy of the repo and the online one (called the "remote")

<a name="commit"/>

## Committing changes
*Creates a snapshot of the status of every tracked file in the repo*
1. ```git add --all``` - tells git to track any newly-added files
2. ```git commit -m "Some useful message about what changes are contained in this commit"```
3. ```git push``` - uploads your changes to the remote repo (i.e. on Github)

<a name="branch"/>

## Making branches
*To do new work without messing up working code on another branch*
1. ```git branch branchname``` - make a new branch locally
2. ```git checkout branchname``` - switch to editing the new branch (these two commands can be accomplished with the shortcut: ```git checkout -b branchname```)
3. Commit (```push``` will fail because the branch isn't yet linked to any corresponding branch on the remote) - do point 4:
4. ```git push -u origin branchname``` - shorthand for ```git push --set-upstream origin branchname```; sets the remote branch which the local should interface with. ```origin``` is the remote name

<a name="merge"/>

## Merging branches 
*To merge finished, working code into another branch*
1. Make sure all changes on both involved branches are committed and pushed - use ```git status``` to check
2. ```git checkout receivingbranch``` - merge is a "receiving" operation
3. ```git merge otherbranch``` - merges changes into ```receivingbranch```
4. ```git push```

<a name="delete"/>

## Deleting branches 
*Delete a branch once it's merged to avoid clutter abnd confusion*
1. ```git push -d origin branchname``` - delete the branch on the remote
2. ```git branch -d branchname``` - delete the local copy
