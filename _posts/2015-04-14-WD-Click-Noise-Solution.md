---
layout: post
category: "System"
title: "WD hdd Click Noise Solution"
tags: [System]
---

# WD hdd Click Noise Solution
西数硬盘咔嚓响，C1门，磁头复位  
The annoying noise of Western Digital Black hard disk, with its so-called "intelligent" Advanced Power Management (APM) feature.

## For Windows
Just use CrystalDiskInfoPortable, and configure it to disbale APM and run it in background.

## For Debian
<!--more-->
- `sudo apt-get install hdparm`
- `sudo hdparm -B /dev/sda` to query the actual APM value of /dev/sda
- `sudo hdparm -B 255 /dev/sda` to disable APM for /dev/sda for now
- `sudo vi /etc/hdparm.conf`, uncomment these 2 lines: `apm = 255`and `apm_battery=255`  
> Modifying /etc/hdparm.conf will assure to disable APM every time the machine Power On, but the state will not last when resuming from suspend or hibernate
- To disable APM when resuming from suspend or hibernate, add a script "hdparm_set" in /lib/systemd/system-sleep/, and add `x` permission for it  
```
#!/bin/sh
hdparm -B 255 /dev/sda
```
> Actually, according to the man page of systemd-suspend.service, the excutables in /lib/systemd/system-sleep/ will be run before and after suspend and hibernate
