# Create daily TS3 server snapshot from Alpine linux Docker image
I run TS3 server on TeamSpeak docker image on my NAS's Docker.

The backup script is run from another container because I don't want to open telnet on the nas itself.
For that I use vanilla minimal Docker image based on Alpine Linux.

Script uses TeamSpeaks serverquery through telnet



##installation

###images for TeamSpeak and Alpine

- https://registry.hub.docker.com/_/teamspeak/
- https://registry.hub.docker.com/_/alpine/

###mounting the storage for persistence for alpine
- alpine is mounted via /mnt to your preferred host path

###changes in the script for your use

- replace SERVERADMINPASSWORD for serveradmin password
  - line 9 should look something like     `echo "login serveradmin 1a2B3c4"`
- replace IPTOYOURTS3SERVER for the IP to your TS server
  - line 23 should look something like     `) | telnet 192.168.1.1 10011 >/mnt/output.txt`
- the script assumes usage of alpine docker image which mounts to the host volume via /mnt
- if you are using different place to run your script, change the paths accordingly

###setting up the alpine
- run `crond -b` in the terminal to run cron
````
/mnt/scripts # ps                                                                                              
PID   USER     TIME  COMMAND                                                                                   
    1 root      0:00 /bin/sh                                                                                   
  312 root      0:00 crond -b                                                                                  
  430 root      0:00 ps           
````
- copy the modified `createTsBackup` into `/etc/periodic/daily`  for daily created backup
````
/mnt/scripts # ls /etc/periodic/daily/                                                                         
createTsBackup
````