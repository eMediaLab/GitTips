## Helpful things to know...


### Begin with Config

User settings are stored in multiple locations: Global, System, and Local. 

**List Configs...**

```
git config --local -l

git config --system -l

git config --global -l 

```

**Config Locations**

```
~/.gitconfig // global

.git/config // local

usr/local/etc // system
```


**Edit Config**

```
git config --edit // Local

git config --global --edit

git config --system --edit
```


**Update Settings**

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

git config --get remote.origin.url // show just the URL
```



**Useful Config Settings (global)**

```
[core]

 editor = code // Visual Studio Code

 autocrlf = input 

 excludesfile = /Users/EML/.gitignore\_global // add files you ALWAYS want to ignore: i.e. .DS\_Store

[remote "origin"]

 url = [https://github.com/eMediaLab/repoNameHere.git](https://github.com/eMediaLab/demo.git) 

 fetch = +refs/heads/\*:refs/remotes/origin/\* 
```
___

## Remotes...

CD into your repo directory, then...

**List Remotes / Branches**

```
git remote -v

git branch -r // will list remote branches
```


**Remove a remote**

```
git remote rm \<location\> \<branch\> // or simply...

git remote rm origin
```

**Add a remote** 

```
git remote add \<name\> \<url\>

git remote add origin https://github.com/eMediaLab/repoName.git
```

**Note:** When adding a remote, use https NOT git@github.com:eMediaLab/repoName.git (SSH)

**Reasoning:**

* https is less likely blocked via firewall.
* Easier setup and works on the widest range of networks/platforms.
* SSH does not support the credential.helper which will cache your un/pw via https.


**Force remove GIT tracking**
```
rm -rf .git // make sure Terminal is pointing to the repo root
```

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


## Workflow

**Fetch**

```
You may want to test using Fetch (instead of pull). This allows to check work before merging! Use fetch and then checkout a branch to see the data.

git fetch // Downloads refs but doesn't merge 

git checkout origin/master // Switch to see fetch data. Works for any remote: origin/branch2, Alias/branch3, etc.
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

For every repo include a Readme, License (if applicable), and a .gitignore file




## Git Workflow

Brief overview of essential commands...

**Simple:** Create Repo on GitHub first, then clone to mac. This way the directory settings are automatically correct for the remote you want to push to. Or...

###Create a new repository on the command line

* echo "\# fix" \>\> README.md
* git init
* git add README.md
* git commit -m "first commit"
* git remote add origin https://github.com/eMediaLab/repoName.git
* git push -u origin master

### Push an existing repository from the command line

* git remote add origin https://github.com/eMediaLab/repoName.git
* git push -u origin master

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