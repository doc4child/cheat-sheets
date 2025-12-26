# OMV 8

User Rufus to burn ISO to usb naming this USB1 (Use USB- blue 32 gb usb3 for faster interface)

Use the 16gb calling this USB2 (scandisk minimum profile/small sticking out to install it on)

give machine name
domain name
root password remember

Once installed boot into the OMV8 by usb2 boot drive

login as root

do "ip a" command to find out the ip address assigned to wifi interface.

if IP is not assigned use "omv-firstaid" to configure network interface. (as root)
use IP add to open the omv web interface from different computer on the same network

## Initial settings on first login
Right upper corner person icon click on it.
Settings dark theme
Select dashboard widgets.

## install upgrades from the dashboard select upgrades and install them, it might take a while.
https://www.youtube.com/watch?v=1QmA_JiWzG8


## Install the omv extras
login using ssh root@192.168.1.xx
wget -O - https://github.com/OpenMediaVault-Plugin-Developers/packages/raw/master/install | bash


## Docker Portainer install
https://www.youtube.com/watch?v=y-OWVwpoGow&list=PL7IO8NXt9zRmNB43lCvDZwK2tsaPpswwC
