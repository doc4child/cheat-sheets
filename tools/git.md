# Git Commands

## Basic commands
COMMAND | DESCRIPTION
---:|:---
`git config --global user.name "John Doe"` | Set username
`git config --global user.email johndoe@example.com` | set email
`git config --global init.defaultBranch main` | Setup default branch to main instead of master
`git config --list`| git configuration

`git remote set-url origin git://new.url.here` | this is how you set remote url -Instead of removing and re-adding, you can do this
`git remote remove origin` | to remove remote 

## Use existing ssh keys

If you have a copy of your ssh keys (e.g., on a USB stick) then simply copy the key files to the ~/.ssh/ directory.

E.g.,

`cp /path/to/my/key/id_rsa ~/.ssh/id_rsa`
`cp /path/to/my/key/id_rsa.pub ~/.ssh/id_rsa.pub`
## change permissions on file
`sudo chmod 600 ~/.ssh/id_rsa`
`sudo chmod 600 ~/.ssh/id_rsa.pub`
## start the ssh-agent in the background
`eval $(ssh-agent -s)`
## make ssh agent to actually use copied key
`ssh-add ~/.ssh/id_rsa`




## Using GitHub with SSH (Secure Shell)
Last Updated : 21 Apr, 2020
Secure Shell (SSH) Protocol facilitates the communication among systems in an unsecured network by providing a secure channel over it. It safeguards the connection to remote servers enabling user authentication.

Using SSH, you can connect to your GitHub account eliminating the need of giving username and password each time you push changes to the remote repository. The integration process involves setting up SSH Keys within both the local and remote systems.

Connect to GitHub using SSH
Note: If you already have an existing SSH key, you can skip step 1 and go to step 2. You can verify the same by listing all the existing keys using the command:

 $ `ls -al ~/.ssh `
Steps to connect GitHub to SSH :

#### Step 1: Generate SSH Key on Local System

Launch Terminal / Git Bash.
Paste the below command and substitute your GitHub email address:
 $ `ssh-keygen -t rsa -b 4096 -C "your_email@example.com" `


Press Enter when prompted “Enter a file in which to save the key”.
Type a passphrase of your choice.
Verify creation of SSH Key:
 $` ls -al ~/.ssh `


#### Step 2: Add SSH Key to SSH Agent

Initiate ssh-agent:
 $` eval "$(ssh-agent -s)" `
If your key is generated with a different name, replace id_rsa in the command below:
 $` ssh-add ~/.ssh/id_rsa `


#### Step 3: Add the SSH Key to your GitHub Account

Copy key to the clipboard:
WINDOWS
$ `clip < ~/.ssh/id_rsa.pub`

LINUX
`cat ~/.ssh/id_rsa.pub`

OR

$`sudo apt-get install xclip`
$ `xclip -sel clip < ~/.ssh/id_rsa.pub`

MAC
$ `pbcopy < ~/.ssh/id_rsa.pub`
Open the GitHub website and log in to your account. Go to the settings page from the menu in top right corner.
Select “SSH and GPG keys” from the sidebar and click on “New SSH key” option.

Add relevant title in the “Title” field and paste the SSH key in the “Key field“.
Now, click on “Add SSH key“.
Step 4: Test the SSH Connection

Launch Terminal / Git Bash.
Type:
 $ `ssh -T git@github.com `


Connection is established if you are prompted with the following message:
Hi {username}! You’ve successfully authenticated, but GitHub does not provide shell access.




