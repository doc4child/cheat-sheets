# Oracle cloud

## Create a virtual machine

## assign a fixed ip address
https://www.youtube.com/watch?v=_9FWri0a9Bo

## open ports
https://www.youtube.com/watch?v=SO11Og_WgSs&t=262s

internet gateway > security list > add Ingress rules
80, 443, 81 for npm


## install docker

This example downloads the script from get.docker.com and runs it to install the latest stable release of Docker on Linux:

### use script to do full install
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh


### install prerequisite for rootless setup 
sudo sh -eux <<EOF
# Install newuidmap & newgidmap binaries

apt-get install -y uidmap
EOF

### install rootless script
dockerd-rootless-setuptool.sh install

Once done export variables:

export PATH=/usr/bin:$PATH
export DOCKER_HOST=unix:///run/user/1001/docker.sock
