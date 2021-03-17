# Installing teamspeak.

## Requirements

N/A

### Semi-automatic installation :

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

### Manual  installation :
