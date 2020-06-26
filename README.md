# Debian-upgrades-instruction

It is very important to keep your os up-to-date. Before upgrading your system, it is recommended that you take a few steps. 

1.Full Backup.

Clonezilla provide the most comfortable and reliable way to make full system backup. 
Here you can find a detailed instruction how to do that: https://clonezilla.org/clonezilla-live-doc.php

2.Check the status of all packages.

Te most reliable way is to type:
#dpkg --audit
or
#dpkg -C

Other commands:
#dpkg -l | pager
#dpkg --get-selections "*" > ~/curr-pkgs.txt

It is desirable to remove any holds before upgrading.  If you changed and recompiled a package locally, and didn't rename it or put an epoch in the version, 
you must put it on hold to prevent it from being upgraded. The “hold” package state for apt can be changed using:
#echo package_name hold | dpkg --set-selections
    
Replace hold with install to unset the “hold” state.If there is anything you need to fix, it is best to make sure your APT source-list files still refer to stretch. 
But in practise, even if you have missing or broken packages, it will not causing eny errors during upgrading.

3. Check your kernel version 
#uname -a

4. Check APT source-list files

Before starting the upgrade you must reconfigure APT's source-list file - /etc/apt/sources.list.  If your folder /etc/apt/sources.list.d/ is not empty (like mine) you need to rewrite the files inside this folder too. 

4.1 Disable the previously existing “deb” lines by placing a hash sign (#) in front of them. 

4.2 Add new sources:

#deb cdrom:[Debian GNU/Linux 10.0.0 _Buster_ - Official amd64 DVD Binary-1 20190706-10:24]/ buster contrib main
#deb cdrom:[Debian GNU/Linux 10.0.0 _Buster_ - Official amd64 DVD Binary-1 20190706-10:24]/ buster contrib main
#Line commented out by installer because it failed to verify:
#deb http://security.debian.org/debian-security buster/updates main contrib
#Line commented out by installer because it failed to verify:
#deb-src http://security.debian.org/debian-security buster/updates main contrib
#buster-updates, previously known as 'volatile'
#A network mirror was not selected during install.  The following entries
#are provided as examples, but you should amend them as appropriate
#for your mirror of choice.
#deb http://deb.debian.org/debian/ buster-updates main contrib
#deb-src http://deb.debian.org/debian/ buster-updates main contrib
deb http://deb.debian.org/debian buster main contrib non-free
deb-src http://deb.debian.org/debian buster main contrib non-free
deb http://deb.debian.org/debian-security/ buster/updates main contrib non-free
deb-src http://deb.debian.org/debian-security/ buster/updates main contrib non-free
deb http://deb.debian.org/debian buster-updates main contrib non-free
deb-src http://deb.debian.org/debian buster-updates main contrib non-free


5. Update your packages

First the list of available packages for the new release needs to be fetched. This is done by executing: 
#apt update

Official debian upgrade guide suggest to type "# apt autoremove" command after "apt update". But be carefull! This command can crash your system by deleting important parts of it. It is strongly recommended that you use the /usr/bin/script program to record a transcript of the upgrade session. Then if a problem occurs, you will have a log of what happened, and if needed, can provide exact information in a bug report. To start the recording, type:

#script -t 2>~/upgrade-busterstep.time -a ~/upgrade-busterstep.script

Remove the packages that have been previously downloaded for installation: 
#apt clean


6. Minimal system upgrade

In some cases, doing the full upgrade (as described below) directly might remove large numbers of packages that you will want to keep. 
That's why it is better to make a two-part upgrade process: first a minimal upgrade to overcome these conflicts, then a full upgrade.

#apt upgrade

Some debian users prefer apt-get upgrade because this command seems to them more carefull. Using "apt-get upgrade" you can install only new version of packages which is already presented in the system.
"apt-get upgrade" command  has the effect of upgrading those packages which can be upgraded without requiring any other packages to be removed or installed. Using apt you can download and install ALL new packages for your debian system upgrade. 
I prefer this way. 

7. Full upgrade

Execute:
#apt full-upgrade

This command only takes affect if you change system version from "Stretch" to "Buster". If you want just update old buster version "apt full-upgrade" return nothing. You already updated your system with "apt upgrade".  
    
8. Upgrade your kernel and related packages

I suppose  you already have a "linux-image-*" metapackage. These metapackages will automatically pull in a newer version of the kernel during upgrades. You can verify whether you have one installed by running:

#dpkg -l "linux-image*" | grep ^ii | grep -i meta

If you have metapackage new version of kernel will be installed automatically. If not you can follow the steps from official debian upgrade guide: 

https://www.debian.org/releases/buster/mips/release-notes/ch-upgrading.en.html

You can find here all necessery information about upgrade process. 


9. Purge removed or obsolete packages

apt purge $(dpkg -l | awk '/^rc/ { print $2 }')

Be carefull during this steps! I decided not run this command. 

Reboot and enjoy your new system!





    






