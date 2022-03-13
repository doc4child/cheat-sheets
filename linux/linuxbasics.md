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





