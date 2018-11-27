# Helpful things to know...


### Customize Git Settings  
Git user settings are stored in multiple locations: **Global**, **Local**, and **System**. To get the most out of Git, take some time to customize your settings. 

For detailed info on customization, visit [git-scm.com](https://git-scm.com/book/en/v2/Customizing-Git-Git-Configuration).

## Quick Commands

**List Configs...**  
Commands to pull up each individual git config settings... 

```
git config --global -l    // Global
git config --local -l     // Local
git config --system -l    // System

```

**Config Locations**  
Location of each git config file on the Mac...

```
~/.gitconfig   // Global
.git/config    // Local
/usr/local/etc  // System

```


**Edit Config**  
Command to quickly edit git config settings...

```
git config --global --edit   // Global
git config --edit            // Local
git config --system --edit   // System

```

**Manual way to Update Specific Config Settings**

```
git config --global user.name "Name Here"
git config --global user.email 

// Change global to system or leave out to set for the current repo
```

**Check Settings**

```
Examples...

git config user.eMail
git config user.name
git remote show origin
git config --get remote.origin.url    // show just the URL
```



**Useful Git Config Settings (Global)**  
Global git settings are a conveninent way to set preferences for all repositories. You can always override a global setting by defining a local git configuration (see above).  

Some helpful Global setting suggestions are listed below. To edit the Global config setting, use this command: **git config --global --edit** 

```
[core]

editor = code      
// Set the default text editor to your preferred text editor 
// The example above sets the editor to Visual Studio Code.

autocrlf = false  // Turns off carriage return line feed injection
safecrlf = false  // Turns off carriage return line feed warnings
                  // Helpful if working with mixed Windows / Mac projects


```
**Note:** 
After much research, I found the **autocrlf** and **safecrlf** settings above useful for my specific working situation ( edits being made to the same file on both a Mac and Windows machine ). If you aren't experiencing CRLF errors then you probably don't need to worry about adding these settings. 
  
This article provides helpful background on the difference between Mac and Windows when it comes to editing text, and how that sometimes leads to a headache when using version control: [Mind the End of Your Line](https://adaptivepatchwork.com/2012/03/01/mind-the-end-of-your-line/) 


## Fetch

### Pull Down and Inspect Changes  

**Fetch: Pull Down Remote Changes**  
If you want to pull down a remote branch, use Fetch. This will update your local repo with the latest remote activity, but it doesn't merge it to any branch. 


```
git fetch  
// Downloads commits, files, and refs (i.e.  branches) but doesn't merge 

```

**Checkout: Inspect Changes**  
After fetching you can inspect any changes made to remote branches.  

```
git branch -r
// List all remote branches that are now available locally to checkout

git checkout origin/dev
// Switches to the remote dev branch (available locally after fetching)
// Replace "dev" to switch to another remote branch
// For example: git checkout origin/someOtherBranchName

```

> **Note:**   
> When you checkout a remote branch, you may see a 'detached HEAD' warning. This isn't anything to be overly concerned about. It simply means... 

> * You are now looking at the remote version of the branch (locally)
> * You are now in a "Read Only" mode 


## Merge Updates

### Update a Local Repo after Fetching

**Merge: Remote Changes to Local Branch**  
If everything appears ok after fetching, checking out, and inspecting any remote changes (see steps above), ...you can then merge the remote branch to your local copy.  

```
git checkout master
// Checkout the local branch you want to merge into (in this case master)

git merge origin/dev
// Merges the remote dev branch into master 
// Replace "dev" to merge another remote branch to master
// For example: git merge origin/anotherRemoteBranchName

```

## Working with Remote Repos

**List remote URLs** (if configured)  
To list the remote URL(s) for your repository, cd to the root level of your repo and then type...

```  
git remote -v

```

**Remove a remote**

```
git remote rm <location> <branch> // or simply...  
Example: git remote rm origin

```

**Add a remote** 

```
git remote add <remote alias/name> <url>  
Example: git remote add origin https://github.com/accountName/repoName.git

```

**Note:** When adding a remote, use https **NOT** git@github.com:accountName/repoName.git (SSH)

> **Reasoning:**

> * https is less likely blocked via firewall.
> * Easier setup and works on the widest range of networks/platforms.
> * SSH does not support the credential.helper which will cache your un/pw via https.




**Set Upstream**

```
git push -u origin master

git branch --set-upstream-to=origin/master // This will set the upstream without doing a push first

// After this is set, "git push" is only needed 

// To set the upstream for other branches the process is the same: git push -u origin \<other branch name\>
```

**Force Push**

```
git push -u origin master --force // This will overwrite remote's history, and set upstream
```

**Allow Unrelated Histories**

```
git pull origin branchname --allow-unrelated-histories

git merge origin/master --allow-unrelated-histories // use this if you want to merge a Fetch 

// If your online and local repos have alternate histories, but you know its ok - use this command. 
```


## Work with Multiple Accounts

Global settings for your username or password do NOT cascade to repos created locally if... 

* osxkeychain helper previously saved your username or password
* You have local config settings for each

**Multiple credentials on GitHub**

Git may use the osxkeychain helper, which is a CLI (command line) utility that is sometimes installed with Git (i.e. via HomeBrew). This will save your username/password to your Keychain when you successfully login to GitHub the first time in Terminal via HTTPS. After logging in the first time, Git will then use the saved info. 

**ISSUE:** You need to Log in under another account, outside of the default/saved keychain credential.

**Solution:** 

1. Update your local repo username and password using git config
2. Erase the saved keychain so Terminal doesn't try to default to that account

## To Reset stored PW in Terminal for your main GitHub Account...

**Use the osxkeychain helper!**

$ git credential-osxkeychain \<get|store|erase\> 

// use get, store, or erase - then specify host, protocol and hit return. 

host=github.com

protocol=https

[Press Return]

---

## Best Practice!...

For every repo include the following essential files... 

* Readme
* License (if applicable)
* .gitignore file



## Simple Way to Create a New Repo

Three steps:  

* Visit [GitHub](http://www.github.com) 
* [Create a new repository](https://help.github.com/articles/create-a-repo/)
* [Clone to your desktop](https://help.github.com/articles/cloning-a-repository/) ( Use HTTPS protocol, not SSH )  

## Manual way to Create a New Repo  
An alternative to creating the repo on GitHub and cloning down to the desktop, is to create the repo on the desktop and then push to GitHub. 

**The first step:** From the command line type `git init <FolderName>`          

The command above will create and initialize a folder with Git version control tracking enabled (replace brackets and folder name with your own). 

**Note:** "git init" without a folder name will initialize whatever directory you are currently pointing to in Terminal, with version control. Careful with this! If the directory you are pointing to already hase version control enabled, the "git init" command will overwrite any previous tracking history!  

```
git add README.md  
git commit -m "first commit"  
git remote add origin https://github.com/accountName/repoName.git  
git push -u origin master

```

## Version Control Tips

**Force remove GIT tracking**
```
rm -rf .git // make sure Terminal is pointing to the repo root
```

### Push an existing repository from the command line

```
git remote add origin https://github.com/eMediaLab/repoName.git  
git push -u origin master

```

**ISSUE:**

You may receive an error if pushing a locally created for first time to a repo that already has activity. You may also receive an error if you are pulling a repo for first time to a local repo that already has activity (i.e. a different commit history).

**Solutions:**

1. Don't initialize the online repo with anything (e.g. A ReadMe, .gitignore, etc.) This way, the first push will not try to overwrite any existing history online. The minute you create a ReadMe, .gitignore, etc. online, a history trail begins.
2. Don't begin a local commit history, if the online repo has activity. Similar to the 1st solution, varying version control history will lead to errors. Instead, pull down any online content first - then begin the local commit history.
3. Edge Case: Files were created online, and commit history is also present in local work. Since the online repo and local repository both have varied histories, a push or pull will error. There are a number of ways to merge the two and avoid errors.
  * Use the force (can be dangerous)! 

    git push -u master origin --force 
    
// Using "--force" will overwrite the entire repo online (and set the default upstream)



## Potential GIT Errors

Trying to push without a commit history

**error:** src refspec master does not match any.

**error:** failed to push some refs to 'https://github.com/eMediaLab/someRepoName.git'

**Solution:** Begin a commit history

**Definition:** a "ref" is equivalent to a commit/record/SHA-1 in your history 

Trying to push local commits to a repo that already has a commit history

![](resources/192A7B2E1CD740A702021F8BB3BC2630.png)

**Solution:** Fetch and Merge, or pull the remote data and override the error...

### Allow Unrelated Histories

git pull origin branchname --allow-unrelated-histories

git merge origin/master --allow-unrelated-histories 

// use this if you want to merge a Fetch 

// If your online and local repos have alternate histories, but you know its ok - use this command. 

Trying to pull a remote repo for the first time, without the upstream set and with no common commits

![](resources/82FBA04BD4E6F14774F109C01F885BCC.png)

**Solution:** See "Allow Unrelated Histories" above... and Set Upstream below!

**Set Upstream**

git push -u origin master

git branch --set-upstream-to=origin/master // This will set the upstream without doing a push first

// After this is set, "git push" is only needed 

// To set the upstream for other branches the process is the same: git push -u origin \<other branch name\>