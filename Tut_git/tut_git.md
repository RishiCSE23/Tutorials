# 1. Introduction

## 1.1. Installing Git and requirements

__1. Let the git know who you are__
```
git config --global user.name {USER_NAME}
git config --global user.email {USER_EMAIL} 
```

__2. Prepare the code base folder__ 
```
git init   # creates an invisible folder .git holding all files requires for management 
```

__3. Staging file__
```
git add . # adds the files to staging (temporary area)
```

__4. Commit changes__ 
```
git commit -m "DESCRIPTIVE STRING"
```
__5. Verify the log__<br>
```
git log
```
output 
```
commit 8478a89aeca7a341ab467cee245182aae626f18c (HEAD -> master)
Author: USER_NAME <USER_EMAIL>
Date:   Mon Jan 9 14:20:40 2023 +0000

    {COMMIT DESC}
```

## 1.2. Git environments 

Git uses 3 environments to manage the code base 
1. __Working Environment__: This area with active updation, where the developer writes codes.
2. __Staging Environment__: This is a temporary area where the stable codes are included to be commited (`git add {--all | A | .}`).
3. __Commited Environment__: Here a staged environment is commited (`commit -m "{COMMIT_MSG}"`) with a commit message, and a log entry is created with a new Hash.  

### 1.2.1. File Stages
* __Tracked__: Files that exist in the previous snapshot (commited)
    * __Unmodified__: Tracked file that are not changed since the last commit.
    * __Modified__: Tracked files that have been changed since the last commit.
    * __Staged__: File that has gone through Staging.
* __Untracked__: Files that are added since the last commit  

### 1.2.2. Status
the `git status` command shows the changes within the PWD. 

### 1.2.3. Restore
If you want to rewind the changes done by `git add .` command, use the `git restore [--staged {FILE_NAME}] | .`. 

## 1.3. Ignoring Files

### 1.3.1. Local Ignore Files
There are files containing sensetive info, personal notes or system files that are not to be shared publicly or to the production. This files are to be ignored. 

Create a `.gitignore` file populate with the list of file names or regex that you want to ignore. 
* `.foo         # this is a hidden file`
* `foo/         # this is folder`
* `foo.bar      # this is a file`
* `**/*-todo.md # regex: anyfile of md type ends with 'todo' anywhere`

### 1.3.2. Global Ignore Files
Ignore files can also be configured at the global level using `git config --global core.excludesfile FILE_NAME`

### 1.3.3. Crearing the Cache
`git rm -r --cached .` 


## 1.4. Renaming and Deleting files

### 1.4.1. Deleting Files
* When a file gets deleted from the code base, Git captures the change. 
* This can be observed by the `git status` commnad.
* This change can be staged using the `git add .` command or the could be reverted using the `git restore .` command
* the `git rm FILE_NAME` command deletes the file and automatically moves the deletion action to staging (thus, no need to do `git add .`) 
* To restore a file deleted using the `git rm` command:
    * First move the files from staging to working area: `git restore -S .`
    * Then resotre the file within the working area: `git restore .`
    
### 1.4.2. Rename Files
* When a file gets renamed (e.g., foo.txt to bar.txt)by standard OS operations (e.g., `rem` or `mv` command). Git registers this action as _Deleting foo.txt followed by Creating a bar.txt_.
* The above can be observed by the `git status` command after renaming a file.
* The `git restore .` command would bring the foo.txt back. Then both foo.txt and bar.txt would exist and the bar.txt has to be removed manually (i.e., rm or del). 
* The `git mv OLD_FILENAME NEW_FILENAME` does the staging automatically.
* Undoing the change could be done in two ways 
    * `git restore` as used in case of deletion 
    * revert the change `git mv NEW_FILENAME OLD_FILENAME`

## 1.5. Differences

* `git diff` command lists all the changes to the code base ("+" means added, "-" means removed)
* VSCode SCM tool has a better way to show the changes. 
* To verify differences specific to a commit 
    * Fist get the commit hash using `git log [--oneline]`
    * use the hash to compare differences using `git diff COMMIT_HASH`

## 1.6. Changing History 

### 1.6.1. Ammend 
Git ammend lets the user change an already exisiting commuit for simple fixes without having to create a new commit. 
* `git commit -amend` : Ammends changes to the last commit. This will open the default code-editor to let you change the commit message. 
* `git commit -am 'AMMENDED_MESSAGE'`: Ammend changes to the last commit with a new commit message (i.e., useful to correct typos in the commit message)
* `git commit -amend --no-edit`: ammends the last commit keeping the last commit message. 

### 1.6.2. Reset
`git reset COMMIT_HASH` lets the user reset the entre codebase to a previously commited state, identified by its commit_hash value.
* A virient of the rest command called the __Hard Reset__ done using `git reset --hard COMMIT_HASH` which removes any commit state that occurs after the stated commit state and removes the files associated with it.
    
### 1.6.3. Rebase
* Rebasing allows commits from a branch to be applied on another branch. 
* Syntax: `git rebase [--interactive] [ {BRANCH_NAME/COMMIT_HASH} | {HEAD~{N}} | --root]`
    * `--interactive`: Lets the default text editor to make changes (recommanded)
    * `HEAD~{N}` : instead of branch name use the Head pointer to roleback N commit steps.
    * `--root` : This will open the default code editor to manipulate the commits 
        * pick: a commit instance. the order can be changed. 
        * squash: merge multiple commits into one commit. 
        * fixup: Merges commits like squash but doesn't use the old commit message 
        

## 1.7. Branches 

### 1.7.1. Switching 
* `git branch` : shows the current branch 
* `git switch -c BRANCH_NAME`: Switches the branch from current (`-c`) to anohter branch `BRANCH_NAME`. If the `BRANCH_NAME` does not exist, git will create a new brach of the same name. 

### 1.7.2. Merging 
* `git merge BRANCH_NAME`: Merges the changes from a branch to the current branch. 

### 1.7.4. Deleting 
* `git branch {--delete | -d | -D} {BRANCH_NAME}`: Deletes an existing branch, typically used when a branch is no longer needed.  

### 1.7.5. Git Flow 
The process of the following is called a Git Flow which is used as a best prectice among developers. It is highly recommanded not to make changes in the main branch in a collaborative projects. The main branch is only allowed to be modified if a fix is stable. 
1. __Create__ a branch to test changes for a fix. 
2. Make the required changes to __fix__ an issue. 
3. If the changes are stable (verified by testing) then __Merge__ the branch to the master. 
5. __Delete__ the old branch. 

## 1.8. Merge Conflict

* Merge conflict happens when a main branch has an exisitng segment of code that another branch tries to replace while merging.
* VSCode provides options to choose which of the changes to keep. 



```python

```
