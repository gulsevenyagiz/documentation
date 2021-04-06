---
title: Teamspeak
weight: 2
chapter: false
---

## Installing teamspeak.

## Requirements

RHEL based distro(Centos, Fedora, AlmaLinux etc.)

## Automatic installation
This script puts the sqlite database and the executable of teamspeak in two different places.```/var/local/teamspeak3/ts3server.sqlitedb``` for the database and ```/opt/teamspeak3-server_linux_amd64/``` to be precise. The distinct advantage of this approse is that it makes updating teamspeak in the future a lot easier. 
###

Clone the script [good_stuff](https://github.com/gulsevenyagiz/good_stuff). 

Give the script execute rights.

```
chmod 755 manager.sh
```
Make sure the script points to the latest Teamspeak version.

```
nano manager.sh
```
Look for the variable **"readonly TEAMSPEAK_URL"** and update it with the latest release of teamspeak.

Execute the script 
```
./manager teamspeak
```
Check if teamspeak was started successfully
```
systemctl status teamspeak
```

#### Updating teamspeak with the manual approach
Simply change the teamspeak version in ```manager.sh``` to the newest version. The script will take care of the rest without overriding the exsisting database.

## Manual  installation

Get the lastest copy of teamspeak via the [teamspeak website](https://www.teamspeak.com/en/downloads/#server).
```
wget https://files.teamspeak-services.com/releases/server/x.x.x/teamspeak3-server_linux_amd64-x.x.x.tar.bz2
```
Move the zip file to a sensible location, in this guide I'm using /opt/
```
mv teamspeak3-server_linux_amd64-3.13.3.tar.bz2 /opt/
```
Unzip the file and cd into the created directory.
```
tar -xvf teamspeak3-server_linux_amd64-3.13.3.tar.bz2
cd teamspeak3-server_linux_amd64-3.13.3
```
Create a new teamspeak user
```
adduser teamspeak --system --no-create-home --no-log --shell /sbin/nologin
```
Change the ownership of all files to the newly created user.
```
chown -R teamspeak:teamspeak .
```
Start teamspeak to create the initial configuration files.
```
sudo -u teamspeak ./ts3server_startscript.sh start createinifile=1 license_accepted=1
```
Stop teamspeak again
```
sudo -u teamspeak ./ts3server_startscript.sh stop
```
Create a folder for the database and configuration files and move the files.
```
mkdir /var/local/teamspeak/
mv ts3server.ini /var/local/teamspeak/
mv ts3server.sqlitedb  /var/local/teamspeak/
```
Create softlinks back to the 


