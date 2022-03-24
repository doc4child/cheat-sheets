# Linux Cheat-Sheet

## Basic commands
COMMAND | DESCRIPTION
---:|:---
`ls` | list files
`ls -l` | long listing
`ls -a` | list all files including . files
`mkdir` | make directory
`rm -rf` | remove r for recursive and f for force
`man <command>` | manual for particular command
`chmod +x filename` | to make file executable
`sudo chown -R username:groupname ./foldername` |to assign ownership of user and group to folder

## Keyboard shortcuts
COMMAND | DESCRIPTION
---:|:---
`Ctrl+L` | takes the command line at the top of console
`Ctrl+D` | disconnect or exit (to go back to user from root etc)

## System commands
COMMAND | DESCRIPTION
---:|:---
`htop` | lists all processor and memory plus all running processes, F10 to quit
`sudo lshw` | shows system information
`ss -ltpn`| lists all the ports listening
`echo $0` | shows default shell
`bash` | change to bash shell from -sh
`sudo apt-get update && sudo apt-get -y upgrade` | updates and upgrades system
`echo "$SHELL"`| check your shell
`sudo !!` | to run previous command with sudo
`history` | to see history
`!123` | to run no 123 command in history
`clear` | to clear console


## Cloud-init
sudo nano /etc/cloud/cloud.cfg

https://www.youtube.com/watch?v=exeuvgPxd-E



## Aliases in Linux

### To create permanent alias you can add it to .bashrc file like below

```
echo "alias up='sudo apt update && sudo apt upgrade'" >> ~/.bashrc
```

https://fossbytes.com/alias-in-linux-how-to-use-create-permanent-aliases/

### seperate aliases in different file for organization

create ~/.bash_aliases file and have it included in ~/.bashrc

```
touch ~/.bash_aliases

nano ~/.bash_aliases
```
Add aliases here

```
alias up='sudo apt-get update && sudo apt-get upgrade && sudo apt-get autoremove && sudo apt-get autoclean'
```
Then edit ~/.bashrc to include following code if not there already

```
if [ -e ~/.bash_aliases ]; then
source ~/.bash_aliases
fi
```