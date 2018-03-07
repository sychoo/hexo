---
title: Unix NoTE
date: 2018-03-05 13:24:05
tags:
---

Welcome to my Unix NoTE! This is a page that records all the important UNIX commands, bugs and shortcuts during my journey of learning Unix. Check it out!


# Short Cuts
- ### [**Terminal**](#Terminal)

| Key     	 				| Functions            |
| ------------------------- | :--------------------|
| ctrl + h, del, backspace 	| Erase a character(from the right)| 
| ctrl + d 					| Erase a character(from the left)|
| ctrl + w                  | Erase a word |
| ctrl + u  				| Delete a line | 
| ctrl + c   				| Abort execution (SIGINT)| 
| ctrl + d (shell)  		| Terminate program (EOF) | 

# Useful Commands

- ### [**Symbolic Links**](#Symbolic-Links)
```
$ ln -s <source file> <symbolic link>
```
Replace `<source_file>` with the name of the existing file for which you want to create the symbolic link (this file can be any existing file or directory across the file systems). Replace `<symbolic link>` with the name of the symbolic link.

- ### [**Dates**](#Dates)
`date` command displays and sets the system and date. The date utility displays the time and date known to the system. A user working with root priviledges can use date to change the system clock.

Here is a table of selected field descriptors for `date`

| Descriptor | Meaning 							  		  |
| ---------- | :----------------------------------------- |
| %A 		 | Unabbreviated weekday - Sunday to Saturday |
| %a   		 | Abbreviated weekday - Sun to Sat | 
| %B 		 | Unabbreviated months - January to December | 
| %b   		 | Abbreviated months - Jan to Dec |
| %c         | Date and time in default format used by date|
| %D  		 | Date in mm/dd/yy format | 
| %d 	 	 | Day of the month - 01 to 31 | 
| %H         | Hour - 00 to 23 | 
| %I      	 | Hour - 00 to 12 |
| %j  		 | Julian date (day of the year - 001 to 366)|
| %M         | Minutes - 00 to 59 | 
| %m 	  	 | Month of the year - 01 to 12 |
| %n 		 | Newline Character | 
| %P 	     | AM or PM | 
| %r 	 	 | Time in AM/PM notation | 
| %S 		 | Seconds - 00 to 60 (the 60 accomodates leap seconds)|
| %s 		 | Number of seconds since the beginnning of 1/1/1970 |
| %T 		 | Time in HH:MM:SS format |
| %t  		 | TAB character |
| %w      	 | Day of the week - 0 to 6 (0 = Sunday) | 
| %Y  		 | Year in four-digit format (e.g. 2014) |
| %y 		 | Last two digits of the year - 00 to 99
| %Z   		 | Time zone (e.g. PDT)

Usage 
```bash
#!/bin/bash
DATE=$(date +"%Y-%m-%d_%H%M")
raspistill -vf -hf -o /home/pi/camera/$DATE.jpg
```

# Bugs

- ### [**Debian Shutdown Delay**](#Debian-Shutdown-Delay)
```
a stop job is running for session c1 of user debian-gdm
```
This problem occurred when I shutdown the Kali Linux VM that was running on my MBP. The VM won't shut down until 1.5 minutes after executing the shutdown command. I solved the problem by killing `NetworkManager` service before shutting down and rebooting the computer. In order to explain this, we need to have an overview of Unix [`runlevels`](https://www.liquidweb.com/kb/linux-runlevels-explained/).

| ID  |Name                               		| Description
| --- |:----------------------------------------| :--------------------------------------------------------------------------|
| 0   |Halt                               		| Shuts down the system.													  |
| 1   |Single-user Mode                   		| Mode for administrative tasks.											  |
| 2   |Multi-user Mode                    		| Does not configure network interfaces and does not export networks services.|
| 3   |Multi-user Mode with Networking    		| Starts the system normally.												  |
| 4   |Not used/User-definable              	| For special purposes.														  |
| 5   |Start the system normally with with GUI  | As runlevel 3 + display manager(X).										  |
| 6   |Reboot                             		| Reboots the system.														  |

For killing the `NetworkManager` process before shutting down or rebooting the computer, we can put a shell script in the folder `/etc/init.d/` and create symbolic links in the folder `/etc/rc0.d/`(shut down) as well as `/etc/rc6.d/`(reboot). The shell script `stop-network-manager` will be as following 
```bash
#!/bin/bash
systemctl stop NetworkManager
```
After putting it in `/etc/init.d/`, run the following commands
```
ln -s /etc/init.d/stop-network-manager /etc/rc0.d/K01stop-network-manager
ln -s /etc/init.d/stop-network-manager /etc/rc6.d/K01stop-network-manager
```
