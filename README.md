# Samba for Docker Containers

Easy to use samba file share server for docker containers.

## Getting Container Image

For the installation you can use docker pull command to get samba container

```bash
docker pull ahmetozer/samba
```

If you want to build in your computer you can use git and docker build

```bash
git clone git@github.com:AhmetOZER/docker-samba.git
cd docker-samba
docker build -t samba .
```

---

## Basic Run

It is easy to use and configure with automated script.

```bash
docker run -it ahmetozer/samba
 ```

## Accessing container network

There is a few option at this point.
First one is 1 docker container and share with docker-proxy with -p argumant.

```bash
docker run -it -p 445:445 ahmetozer/samba 
```

Second one is without port share. Each docker container has a own static IP address and it is accessible only local network.

```bash
docker run -it ahmetozer/samba --ip 172.17.0.80
```

## Share Folders With Samba Container

If you want to share outside of folder you can use -v argumant while creating docker.
-v has a ":" to bind local and container folder. Before the ":" it's your server directory, after the ":" it's your container directory
For example your website files at /var/www directory and you want to share your web folder with samba.

```bash
server:~# docker run -it -v/var/www:/share/web ahmetozer/samba
Hello. You are installing samba server
username » root
Please enter password (at least 8 character) random password 1dM3ltN6 »
Please retype password »
New SMB password:
Retype new SMB password:
Added user root.
Share Name » web
Share Comment » web Directory
Share path » /share/web
Valid users for web » root
Re generating config file
Reloading samba service
```

---

## Container Commands

This script also has a some basic futures.

### Add User

To add new user in samba container

```bash
useradd
```

Example

```bash
36f60553d4aa:/# useradd
username » web
Please enter password (at least 8 character) random password AjNvK3Gb »
Please retype password »
New SMB password:
Retype new SMB password:
Added user web.
Reloading samba service

###   OR
36f60553d4aa:/# useradd root3 mypassword
New SMB password:
Retype new SMB password:
Added user root3.
Reloading samba service

###   OR
36f60553d4aa:/# useradd root5
Please enter password (at least 8 character) random password VsQLHn5C »
Please retype password »
New SMB password:
Retype new SMB password:
Added user root5.
Reloading samba service

```

### Add Share

Add new folder share

Note: You can't add outside of docker container folder to share with samba after first run. If you want to share outside of the container folder, you have to use -v argument to bind Moby folder to container folder.

```bash
shareadd  
```

Example

```bash
36f60553d4aa:/# shareadd
Share Name » newshare
Share Comment » newshare Directory
Share path » /share/newshare
Valid users for newshare » newshare
Re generating config file
Reloading samba service

###   OR
36f60553d4aa:/# shareadd newshare "/share/newshare" "newshareuser" "newshare Directory"
Share Name » newshare
Share Comment » newshare Directory
Share path » /share/newshare
Valid users for newshare » newshareuser
Re generating config file
Reloading samba service

```

### List Shares

You can see your shares in container with sharelist command.

```bash
sharelist
```

Example

```bash
36f60553d4aa:/# sharelist
newshare
root
```

### Show Configure of Share

Show share samba configration.

```bash
shareshow
```

Example

```bash
36f60553d4aa:/# shareshow newshare
[newshare]
              comment = newshare Directory
              path = /share/newshare
              read only = No
              writeable = yes
              browsable = yes
              valid users = newshare
              public = no
              create mask = 0640
              directory mask = 0750
              guest ok = no
```

### Update Samba Configuration

If you add new share without "shareadd" or "useradd" commands, like a add with custom config with nano, you can update configure and reload samba with update-samba.

```bash
update-samba
```

Example

```bash
36f60553d4aa:/# update-samba
Re generating config file
Reloading samba service

```

### Delete Share

If you want to delete share in docker container, you can use sharedel command.

```bash
sharedel
```

Example

```bash
36f60553d4aa:/# sharedel newshare
Deleting file /etc/samba/conf.d/newshare.conf
Re generating config file
Reloading samba service
```

### Reload Samba

To reload samba.

```bash
reload-samba
 ```

---

## Fast Install

Whith fast install you can provide an usernamelist and the config files and then the smb will be setted up for you. This is useful for ussage with docker-compose.

---

Samba share configs are stored at  /etc/samba/conf.d/ in docker container directory.
