---
title: Enable the root user in Mac OS X 
date: 2018-03-24 22:44:53
tags:
---

- ### Hold down the Command (⌘)+ S buttons while turning on the machine, the system will then boot into Single User Mode

- ### After entering the Single User Mode, type the following commands at the prompt:

```bash
/sbin/fsck -fy
/sbin/mount -uw /
launchctl load /System/Library/LaunchDaemons/com.apple.opendirectoryd.plist
passwd root
```

- ### Then you will be prompted to enter a password twice, at the prompt type:

```bash
reboot
```
- ### After rebooting the computer, you will be able to login to you computer using the username and the password that you just set. Enjoy~

[Source](https://sachinparmarblog.com/enable-root-user-password-in-single-user-mode/)
