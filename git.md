---
editor_options: 
  markdown: 
    wrap: 100
---

# Git Commands

## Basic commands

|                                                                      COMMAND | DESCRIPTION                                    |
|-----------------------------------------------------------------------------:|:-----------------------------------------------|
|                                   `git config --global user.name "John Doe"` | Set username                                   |
| `git config --global user.email "29556646+account@users.noreply.github.com"` | Set email                                      |
|                                `git config --global init.defaultBranch main` | Setup default branch to main instead of master |
|                                                          `git config --list` | List git configuration                         |

## Linux: Use existing ssh keys

If you have a copy of your ssh keys (e.g., on a USB stick) then simply copy the key files to the
\~/.ssh/ directory.

```{bash}
# When you are loggged in as a perticular user ~/ refers to /home/username

$ cp /path/to/my/key/id_rsa ~/.ssh/id_rsa`
$ cp /path/to/my/key/id_rsa.pub ~/.ssh/id_rsa.pub`
## change permissions on file
$ sudo chmod 600 ~/.ssh/id_rsa`
$ sudo chmod 600 ~/.ssh/id_rsa.pub`
## start the ssh-agent in the background
$ eval $(ssh-agent -s)`
## make ssh agent to actually use copied key
$ ssh-add ~/.ssh/id_rsa`
```

## For rstudio container exec as bash to setup git

docker exec -it rstudio /bin/bash
cd /home/username
mkdir .ssh

copy id_rsa to host linux /volumes/rstudio/home directory

That directory is /home/rstudio inside the container you should find that file there.
copy that file to container_user's home directory which is /home/container_user/.ssh

cp /home/rstudio/id_rsa /home/container_user/.ssh/

now it is available for authorization for user

from container as root install openssh-client

apt-get install openssh-clientssh

now test connection
ssh -T git@github.com

you may have to create known hosts file and give 777 permissions, once done chmod 644

   cd /home/doc4child/.ssh
   ls
   touch known_hosts
   chmod 777 known_hosts
   
   chown doc4child:doc4child known_hosts




## Linux: Install existing public key from current computer or Github to remote server

```{bash}

# If you are not logged in and want to copy file from current computer to remote computer use following command.

$ ssh-copy-id username@10.10.10.10

OR

$ ssh username@10.10.10.10 "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"

# if you are already logged in and want to import public keys from github, use following command.
# use this when you already have privatekey, public key and ppk key setup for github (coverted on WinSCP-since WinSCP does not accept OpenSSH keys)

$ ssh-import-id-gh <username>
```

## Windows: Use existing ssh keys to setup Git

If you have a copy of your ssh keys (e.g., on a USB stick) then simply copy the key files to the
\~/.ssh/ directory.

Click from git GUI- Repository \> git bash

```{bash}
$ cp '/path/to/Key/id_rsa' /c/Users/username/.ssh/
$ cp '/path/to/Key/id_rsa.pub' /c/Users/username/.ssh/
## change permissions on file
$ sudo chmod 600 ~/.ssh/id_rsa`
$ sudo chmod 600 ~/.ssh/id_rsa.pub`
## start the ssh-agent in the background
$ eval $(ssh-agent -s)`
## make ssh agent to actually use copied key
$ ssh-add ~/.ssh/id_rsa`

```

=======

## Initiate Repository

If it is the first time you are setting up repository files initiate the repository and add origin

```{bash}
# first navigate to the the appropriate folder.
$ git init
$ git remote add origin git@github.com:account/repo-name.git
`git remote set-url origin git://new.url.here` 

# this is how you set remote url -Instead of removing and re-adding, you can do this note that it is git (SSH) address, not https address. Use this after setting up ssh keys for authentication.

Note:--Following command can remove origin
$ git remote remove origin` | to remove remote 

```

## Automating push changes

This can be done by creating a bash script placed preferably in home folder for easy access. Use
following template and save as filename.sh

```{bash}
#!/bin/bash
# if it is first time init the repositoy and add origin
# git init
# git remote add origin git@github.com:account/repo-name.git
# git remote set-url --add --push origin git@github.com:account/repo-name.git
cd /volumes/rstudio/projects/repo-name
$ git remote set-url origin git@github.com:account/repo-name.git
$ git add . && \
$ git add -u && \
$ git commit -m "Updated: `date +'%Y-%m-%d %H:%M:%S'`" && \
$ git push -u origin main

```

## Run file

Assign executable permission to that file, then file can be run to execute that code to push changes
to git.

```{bash}
# gp for git-push (name could be anything, but this convention will keep these files together)
$ chown +x gp-filename.sh
$ ./gp-filename.sh
```

## Detached head

If you have changed files you don't want to lose, you can push them. I have committed them in the
detached mode and after that you can move to a temporary branch to integrate later in master.

```{bash}
$ git commit -m "....."
$ git branch temp
$ git checkout master
$ git merge temp

```

## Git stash

https://git-scm.com/book/en/v2/Git-Tools-Stashing-and-Cleaning


## Using GitHub with SSH (Secure Shell)

Follow this if you are creating SSH key for the first time.

<https://www.geeksforgeeks.org/using-github-with-ssh-secure-shell/>

Last Updated : 21 Apr, 2020 Secure Shell (SSH) Protocol facilitates the communication among systems
in an unsecured network by providing a secure channel over it. It safeguards the connection to
remote servers enabling user authentication.

Using SSH, you can connect to your GitHub account eliminating the need of giving username and
password each time you push changes to the remote repository. The integration process involves
setting up SSH Keys within both the local and remote systems.

Connect to GitHub using SSH Note: If you already have an existing SSH key, you can skip step 1 and
go to step 2. You can verify the same by listing all the existing keys using the command:

```{bash}
$ ls -al ~/.ssh
```

Steps to connect GitHub to SSH :

#### Step 1: Generate SSH Key on Local System

Launch Terminal / Git Bash. Paste the below command and substitute your GitHub email address:

```{bash}
$ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

Press Enter when prompted "Enter a file in which to save the key". Type a passphrase of your choice.
Verify creation of SSH Key:

```{bash}
$ ls -al ~/.ssh
```

#### Step 2: Add SSH Key to SSH Agent

Initiate ssh-agent:

```{bash}
$ eval "$(ssh-agent -s)"
```

If your key is generated with a different name, replace id_rsa in the command below:

```{bash}
$ ssh-add ~/.ssh/id_rsa
```

#### Step 3: Add the SSH Key to your GitHub Account

Copy key to the clipboard: WINDOWS

```{bash}
$ clip < ~/.ssh/id_rsa.pub
```

LINUX

```{bash}
$ cat ~/.ssh/id_rsa.pub
```

OR

```{bash}
$ sudo apt-get install xclip

$ xclip -sel clip < ~/.ssh/id_rsa.pub
```

MAC

```{bash}
$ pbcopy < ~/.ssh/id_rsa.pub
```

Open the GitHub website and log in to your account. Go to the settings page from the menu in top
right corner. Select "SSH and GPG keys" from the sidebar and click on "New SSH key" option.

Add relevant title in the "Title" field and paste the SSH key in the "Key field". Now, click on "Add
SSH key". Step 4: Test the SSH Connection

Launch Terminal / Git Bash. Type:

```{bash}
$ ssh -T git@github.com
```

Connection is established if you are prompted with the following message: Hi {username}! You've
successfully authenticated, but GitHub does not provide shell access.

## Git branches

<https://www.nobledesktop.com/learn/git/git-branches>

Git lets you branch out from the original code base. This lets you more easily work with other
developers, and gives you a lot of flexibility in your workflow.

Here's an example of how Git branches are useful. Let's say you need to work on a new feature for a
website. You create a new branch and start working. You haven't finished your new feature, but you
get a request to make a rush change that needs to go live on the site today. You switch back to the
master branch, make the change, and push it live. Then you can switch back to your new feature
branch and finish your work. When you're done, you merge the new feature branch into the master
branch, and both the new feature and rush change are kept!

For All the Commands Below The commands below assume you've navigated to the folder for the Git
repo.

See What Branch You're On Run this command: git status List All Branches NOTE: The current local
branch will be marked with an asterisk (\*).

To see local branches, run this command: git branch To see remote branches, run this command: git
branch -r To see all local and remote branches, run this command: git branch -a Create a New Branch
Run this command (replacing my-branch-name with whatever name you want): git checkout -b
my-branch-name You're now ready to commit to this branch. Switch to a Branch In Your Local Repo Run
this command: git checkout my-branch-name Switch to a Branch That Came From a Remote Repo To get a
list of all branches from the remote, run this command: git pull Run this command to switch to the
branch: git checkout --track origin/my-branch-name Push to a Branch If your local branch does not
exist on the remote, run either of these commands: git push -u origin my-branch-name git push -u
origin HEAD NOTE: HEAD is a reference to the top of the current branch, so it's an easy way to push
to a branch of the same name on the remote. This saves you from having to type out the exact name of
the branch!

If your local branch already exists on the remote, run this command: git push Merge a Branch You'll
want to make sure your working tree is clean and see what branch you're on. Run this command: git
status First, you must check out the branch that you want to merge another branch into (changes will
be merged into this branch). If you're not already on the desired branch, run this command: git
checkout master NOTE: Replace master with another branch name as needed. Now you can merge another
branch into the current branch. Run this command: git merge my-branch-name NOTE: When you merge,
there may be a conflict. Refer to Handling Merge Conflicts (the next exercise) to learn what to do.
Delete Branches To delete a remote branch, run this command: git push origin --delete my-branch-name
To delete a local branch, run either of these commands: git branch -d my-branch-name git branch -D
my-branch-name NOTE: The -d option only deletes the branch if it has already been merged. The -D
option is a shortcut for --delete --force, which deletes the branch irrespective of its merged
status.

