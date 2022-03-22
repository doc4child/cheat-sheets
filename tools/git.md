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
   chmod 644 known_hosts


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

## QNAP NAS login with SSH

Workaround is to use same public key as your other servers.

For example from ubuntu server copy authorized_keys file to home directory

```{bash}
$ cp ~/.ssh/authorized_keys /home/username/public.key
```

Download with public.key file then import in NAS or another linux server. now copy it as
authorized_key file in the new server.

```{bash}
$ cp /path/to/public.key ~/.ssh/authorized_keys
```

view file to make sure it copied correctly. cat \~/.ssh/authorized_keys

Now you can use same private key for authentication to different server.

===============================================

SSH: How To Set Up Authorized Keys Below is a quick how-to for implementing public / private key
authentication for SSH. This is by no means an exhaustive examination of the subject. \*nix
distributions vary slightly and further research may be needed.

Contents 1 Why use Public Key Authentication? 2 How does it work? 3 Setup Procedures 3.1 Step by
Step Example Why use Public Key Authentication? Public key authentication is considered a more
secure methods of authenticating the Secure Shell than the simple password challenge routine, a
method often broken by brute-force attacks. In addition, public key authentication allows for
automated login routines between machines, thus enabling a range of scripted jobs (think rsync or
port tunneling). It can also simplify the login process without compromising password security.

How does it work? Public key authentication uses a pair of computer generated keys - one public and
one private -- to authenticate between a host and a client. The public key is derived from the
private key. When authenticating, the host machine compares the public key to the private key in
order to verify the veracity of the public key. If the two match, access is granted. Security of the
system is predicated on the security of the private key.

Setup Procedures Generate the needed Public and Private keys on the host. Transfer / append the
public key to the authorized_keys file on the client. Login via Public Key Authentication. Step by
Step Example The below setup description assumes that you are able to run terminal or a terminal
application like Putty, and that you are familiar with basic commands. It doesn't take much.

Let's setup SSH public key authentication between your home computer (hereafter referred to as the
"Host") and your QNAP device (hereafter referred to as the "Client").

1.  Login to the Host via SSH using your preferred terminal application and generate the public /
    private key pair. In terminal type the following at the command prompt:

```{bash}
$ ssh-keygen -t rsa -C "server comment field"

```

Note: the -C switch is not required. It allows you to insert a comment that will appear in the
authorized_keys file. It is helpful for identifying and managing keys within the authorized_keys
file on the Client in the event that you have multiple key logins. Replace "server comment field"
with a machine name, IP address, date, or task name so that you can easily identify where and why a
given key was created.

2.  Execute the command and you should see the following output:

Generating public/private rsa key pair.

Enter file in which to save the key (/Users/UserName/.ssh/id_rsa):

Note: "UserName" is the user account that you have logged into via SSH. Also note that the actual
suggested path may vary slightly depending your system. You should accept the suggested location
unless you have reason to do otherwise.

You will then be prompted for a passphrase that will be associated with this key.

Enter passphrase (empty for no passphrase):

Enter same passphrase again:

The passphrase can be thought of as a password for the private key - it serves as an extra layer of
protection as described below. If you leave this field blank you will generate keys that do not
prompt for a passphrase. In other words, you will be logged in automatically via the secure public /
private key handshake that you are in the process of setting up. It is highly recommended that you
enter a passphrase unless you are setting up automated routines that require automatic login. See
more detail below in "Security Notes."

The keys have now been generated and are stored in the .ssh folder associated with the user account
on the Host machine.

3.  The final steps are to copy the public key to the Client and append it to the authorization_keys
    file. There are a number of ways to do this -- you can copy the file to the Client and then
    append it (I like this method being the relative noob that I am. It involves more steps but is
    the easiest to complete without error. Those proficient with terminal commands will do it all in
    one step from the Host). Alternatively, you can open the id_rsa.pub file via a text editor like
    iv and copy the key text - then open the authorized_keys file on the Client and paste the text
    directly into the file. I will walk you through the noob method.

In the terminal navigate to the folder that contains the newly created keys -- cd
/User/UserName/.ssh and use the "ls" list comand to see what is in that directory. You should see
the following files:

id_rsa id_rsa.pub known_hosts. Type the following at the command prompt:

```{bash}
$ scp id_rsa.pub admin@ClientIPAddress /etc/config/ssh/
```

[admin\@ClientIPAddress](mailto:admin@ClientIPAddress){.email} is the address of your QNAP NAS (the
Client) followed by the path on the Client where the public key needs to reside. You will be ask you
for the password of the "admin" account to login to the Client.

password:

Enter the password to complete secure copy.

4.  On the Client (QNAP NAS) navigate to the /etc/config/ssh folder and "ls" to reveal the contents
    of the directory. You should see your id_rsa.pub file.

5.  Now let's append this file to the authorized_keys file which needs to reside in this directory.
    Do not worry if authorized_keys file is not present. We will create it.

Type the following at the command prompt:

```{bash}
$ cat id_rsa.pub >> authorized_keys
```

Note: You have to cut the key in the file to one line, and add ssh-rsa in at the very beginning.

Example

ssh-rsa
AAAAB3NzaC1kc3MAAACBAPY8ZOHY2yFSJA6XYC9HRwNHxaehvx5wOJ0rzZdzoSOXxbETW6ToHv8D1UJ/z+zHo9Fiko5XybZnDIaBDHtblQ+Yp7Stx...4loWgV=
Set correct permissions

SSH daemon is really peacky about permissions. Create a .ssh folder within your home folder, copy
/etc/config/ssh/authorized_keys to this folder and then make sure you have set your permissions as
follows:

```{bash}
$ chmod 0711 ~ 
$ chmod 0700 ~/.ssh 
$ chmod 0600 ~/.ssh/authorized_keys 
```

That's it. You should now be able to login using key authentication. Logout of the Client and
attempt to login. If you created a passphrase for your id_rsa private key then you will be prompted
for the passphrase. If you left the passphrase field blank when generating the keys then you will be
logged in automatically. The first time you login you may encounter a promoted message like below.

The authenticity of host 'xxx.xxx.xxx.xxx' can't be established. RSA key fingerprint is
XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX. Are you sure you want to continue connecting
(yes/no) ? Type "yes" to continue. After you accept the authenticity of the RSA key, the Client
information is saved in the /Users/UserName/.ssh/known_hosts file. You will not be prompted again
unless you remove this file.

Important If the destination Client is a x86 series model, please execute the following command on
the destination Client (TS-509) to make sure the folder permission has been set correctly. Because
there might be a permission problem in earlier firmware versions in x86 model (e.g. TS-509)

```{bash}
$ chown admin.administrators /mnt/HDA_ROOT/.config
```

Note The procedure is the same when setting up key authentication between two NAS boxes. Simply
follow the steps above substituting a NAS device for the Host and Client as per the above example.

Trouble Shooting O'Reilly provides an excellent reasource for troubleshooting.

Security Notes You must keep your private key secure! Security of public key authentication is
dependent on your ability to secure the private key. You should avoid generating a key without a
passphrase. If you have an unencrypted private key (no passphrase) stored on your workstation and if
your workstation is compromised, your Client machines have been compromised too! If a hacker can
obtain the private key they will have access to any client machines accessible with the public key.
Further, in the event that your machine is compromised you should consider strongly the possibility
that your keystrokes are being logged and therefore any private keys that ar protected by
passphrases have likely been compromised.

In addition, you should never allow root-to-root trust between systems. Unfortunately, the firmware
version of the QNAP NAS series is hardwired with a variant of OpenSSH that only permits ssh login by
the root (admin) user. You are strongly encourage to update the SSH Daemon With OpenSSH as per How
To Replace_SSH Daemon With OpenSSH. You can then setup named accounts for users or roles, granting
as little root access as possible via sudo. You should also limit the "from" access of the public
keys, and if possible only allow specific public keys to run specific commands.

Original Example Here is an example which tells you how to set up authorized_keys between two QNAP
NAS units. For example, we have one TS-209 (10.8.12.209) & a TS-509 (10.8.12.33), and now I want to
make it possible to SSH login to TS-509 from TS-209 without password.

1.  SSH login to TS-209 with a console application (e.g. Putty)

2.  Execute the following command, secure copy the id_rsa.pub from TS-209 to TS-509, and save it as
    "authorized_keys".

```{bash}
$ scp ~/.ssh/id_rsa.pub .ssh/authorized_keys
```

3.  You might see a promoted message like below if this is the first time you are trying to access
    one NAS from another. Simply type "yes" and go to the next step.

The authenticity of host '10.8.12.33'(10.8.12.33) can't be established. RSA key fingerprint is
XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX. Are you sure you want to continue connecting
(yes/no) ? 4. And it will ask you for the password of the "admin" account to login the destination
host (10.8.12.33), just enter it and finish the secure copy command.

\$ password:

5.  That's it! Now you should be able to SSH login to the TS-509 from TS-209 without password, the
    authorized_keys has been saved in the destination host (TS-509) under
    /etc/config/ssh/authorized_keys

Note If the destination host is a x86 series model, please execute the following command in the
destination host (TS-509) to make sure the folder permission has been set correctly. Because there
might be a permission problem in earlier firmware versions in x86 model (e.g. TS-509)

```{bash}
$ chown admin.administrators /mnt/HDA_ROOT/.config
```
