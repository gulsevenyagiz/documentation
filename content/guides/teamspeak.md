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

#### Updating teamspeak with the automatic approach
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
tar -xvf teamspeak3-server_linux_amd64-3.13.3.tar.bz2 /opt/teamspeak3-server_linux_amd64/
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
Create softlinks back to the folder of teamspeak.
```
ln -s /var/local/teamspeak/ts3server.ini /opt/teamspeak3-server_linux_amd64/
ln -s /var/local/teamspeak/ts3server.sqlitedb /opt/teamspeak3-server_linux_amd64/
```

Add filewall rules and restart firewall-cmd
```
firewall-cmd --permanent --zone=public --add-port=9987/udp
firewall-cmd --permanent --zone=public --add-port=30033/tcp
systmectl reload firewalld
```

Create the service
``` 
nano /etc/systemd/system/teamspeak.service 
```
Copy-paste the content
```
[Unit]
Description=TeamSpeak Server Service
After=network.target

[Service]
Type=forking
WorkingDirectory=/opt/teamspeak3-server_linux_amd64/
ExecStart=/opt/teamspeak3-server_linux_amd64/ts3server_startscript.sh start inifile=ts3server.ini
ExecStop=/opt/teamspeak3-server_linux_amd64/ts3server_startscript.sh stop
User=teamspeak
Group=teamspeak
StandardOutput=syslog
StandardError=syslog
SyslogIdentifier=teamspeak

[Install]
WantedBy=multi-user.target
```
Enable teamspeak on boot and start
```
systemctl enable teamspeak
systemctl start teamspeak
```

#### Updating with the manual approach.
Stop teamspeak
```
systemctl stop teamspeak
```
Remove the exsisting teamspeak
```
rm -rf  /opt/teamspeak3-server_linux_amd64
```
Download the new version as descriped in the steps above.

Create new softlinks
```
ln -s /var/local/teamspeak/ts3server.ini /opt/teamspeak3-server_linux_amd64/
ln -s /var/local/teamspeak/ts3server.sqlitedb /opt/teamspeak3-server_linux_amd64/
```
Start teamspeak
```
systemctl start teamspeak
```

