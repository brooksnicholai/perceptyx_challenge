# perceptyx_challenge
Steps to create the container that I did: (I now realize that the "proper way" would be to use a Dockerfile) 

----------------------------------------------------------------------------------

Install Docker for your preferred platform. I did windows:
https://store.docker.com/editions/community/docker-ce-desktop-windows

docker login -u username -p password (use your id and passfor docker hub)

docker search ubuntu:16.04
docker pull wtanaka/ubuntu-1604
docker run -i -t --name demo wtanaka/ubuntu-1604 /bin/bash

sed -i -e 's/us.archive.ubuntu.com/archive.ubuntu.com/g' /etc/apt/sources.list

apt-get update
apt-get install curl
(confirm y)

curl -L -k https://bootstrap.saltstack.com -o install_salt.sh

curl -k https://repo.saltstack.com/apt/ubuntu/16.04/amd64/latest/SALTSTACK-GPG-KEY.pub
 
sh install_salt.sh -P

exit the container

docker commit 8a5f19c6ee30 challenge

docker save challenge > challenge.tar

then zip using 7zip and split for size


----------------------------------------------------------------------------------
This isn't using salt but I also played around with the following which worked to create two containers one with nginx and one with mysql.

docker run --detach --publish 80:80 --name webserver nginx

docker run -d -p 3306:3306 --name mysql-server --env="MYSQL_ROOT_PASSWORD=123456" mysql

----------------------------------------------------------------------------------

# steps to run:

Unzip docker container challenge.7z

run:
docker run -i -t --name nicholai challenge /bin/bash

service salt-master start
service salt-minion start

when tested, salt minion isn't responding would need to figure out how to fix this with containers - probably using a dockerfile

# high level steps I would do: 

checkout git branch which contains salt code to your salt files root folder

apply salt states and  test program

salt states will contain:

install nginx and add php or go support for program

import my sql app code from repo and start nginx

install mysql and start

import mysql DB from repo

git branch will need to have:

salt state files

db from github provided for the challenge 

my sql app code which will: connect to DB, run SQL select, disconnect from DB, and display result

