---
title: System Administrator Manual
date: 2018-05-31 16:45:33
tags:
---

```bash
# Created Thu Apr  5 16:39:19 UTC 2018
# http://www.comptechdoc.org/os/linux/usersguide/linux_ugusers.html

# Default Groups
Administrators : 10000
Developers : 20000

# Change users' passwd
passwd

#  Change users' primary group
sudo usermod --gid <group-name> <user-name> 

# To add a user to a specific group use
sudo usermod -G <group-name> -a <user-name>

# Add user to a group
sudo adduser <user-name> <group-name>

# Create group with group id
groupadd -g <group-id> <group-name>

# list of all the groups
cat /etc/group
groups

# list of all the users
cut -d: -f1 /etc/passwd
cat /etc/passwd

# ID Infomation of a user
id <user-name>

# find user infomation
grep <user-name> /etc/passwd 


# Change user infomation
    # Change General infomation
    sudo chfn <user-name>
    # Change Default Shell
    sudo chsh <user-name>

# Delete users / groups
groupdel <group-name>
userdel <user-name>

# Default umask 
0077

# Sudoer file to execute certain command
leaf ALL=(root) <specific-files>
```
