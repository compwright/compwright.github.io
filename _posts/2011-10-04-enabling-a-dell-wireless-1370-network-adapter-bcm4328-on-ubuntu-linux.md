---
id: 304
title: Enabling a Dell Wireless 1370 network adapter (BCM4328) on Ubuntu Linux
date: 2011-10-04T18:40:07+00:00
author: Jonathon
layout: post
guid: http://jonathonhill.net/?p=304
permalink: /2011-10-04/enabling-a-dell-wireless-1370-network-adapter-bcm4328-on-ubuntu-linux/
categories:
  - Notebook
tags:
  - BCM4328
  - Linux Mint
  - ndiswrapper
  - Ubuntu
---
After three recent virus infections on Windows XP and Windows 7 (including at least one rootkit infection), I turned to <a href="http://www.ubuntu.com" target="_blank">Ubuntu Linux</a> as a safer operating system. Two of the PCs were blessed with Atheros-based wireless network adapters, which are well-supported on Linux. The other laptop, a Dell Inspiron 2200, is blessed with one of those infamous Broadcom chipsets.

Supposedly, the <a href="http://sourceforge.net/apps/mediawiki/ndiswrapper/index.php?title=Dell_Wireless_1370" target="_blank">BCM4328</a> (rev 02) wireless chipset is <a title="b43 and b43legacy Linux wireless drivers" href="http://linuxwireless.org/en/users/Drivers/b43" target="_blank">supported on Linux</a>, but as of Ubuntu Desktop 11.04 and Linux Mint 11, it doesn&#8217;t work reliably. So I turned to the old tried-and-true <a href="http://en.wikipedia.org/wiki/NDISwrapper" target="_blank">ndiswrapper</a> to run the BCM4328 <a href="http://ftp.us.dell.com/network/R115321.EXE" target="_blank">Windows driver</a> under Linux.<!--more-->

## ndiswrapper conflicts

The process was nowhere near as straightforward as I expected. Ubuntu users since 6.04 have experienced with <a title="Module b44 interfering with ndiswrapper upon startup" href="https://bugs.launchpad.net/ubuntu/+source/linux/+bug/214917" target="_blank">ndiswrapper</a> <a title="ssb module breaks BCM4328 with ndiswrapper (regression from 2.6.24-10)" href="https://bugs.launchpad.net/ubuntu/+source/linux/+bug/197558" target="_blank">conflicts</a> with any of the following kernel modules:

  * bcm43xx
  * b43
  * b43legacy
  * ssb
  * b44

## Resolution

  1. Blacklist the conflicting modules: 
        echo -e "blacklist bcm43xx\nblacklist b43\nblacklist b43legacy\nblacklist ssb\nblacklist b44" | sudo tee -a /etc/modprobe.d/blacklist.conf

  2. Install the Windows driver into ndiswrapper: 
        sudo ndiswrapper -i ~/drivers/bcmwl5.inf
        sudo ndiswrapper -l
        sudo ndiswrapper -m
        sudo depmod -a
        sudo modprobe ndiswrapper

  3. Configure nm-applet to load ndiswrapper: 
        echo -e "\nndiswrapper\n" | sudo tee -a /etc/modules

  4. Create a boot script to unload and re-load the conflicting kernel modules in the right order (thanks to <a href="https://bugs.launchpad.net/ubuntu/+source/linux/+bug/197558/comments/14" target="_blank">levmatta</a>): 
      1. Create a file in /etc/init.d/ndiswrapper with this text: 
            #! /bin/sh
            ### BEGIN INIT INFO
            # Provides: ndiswrapper
            # Required-Start:
            # Required-Stop:
            # Default-Start: S
            # Default-Stop:
            # Short-Description: enable to load ndiswrapper
            # Description: enable to load ndiswrapper
            ### END INIT INFO
            
            rmmod b44
            rmmod ohci_hcd
            rmmod ssb
            rmmod ndiswrapper
            modprobe ndiswrapper
            modprobe ssb
            modprobe ohci_hcd
            modprobe b44
            
            ############# end file ############
    
      2. Set the file access permissions: 
            sudo chmod 755 /etc/init.d/ndiswrapper
    
      3. Set this script to run at startup: 
            sudo ln -s /etc/ndiswrapper /etc/rc2.d/S99ndiswrapper

  5. Reboot.

This worked for me on Linux Mint 11, which is based on Ubuntu 11.04. Your mileage may vary.

## References

  * <a href="https://help.ubuntu.com/community/WifiDocs/Driver/Ndiswrapper" target="_blank">Ubuntu ndiswrapper community documentation</a>
  * Ubuntu forum topic &#8220;<a href="http://ubuntuforums.org/showthread.php?t=766560" target="_blank">Broadcom Wireless Setup for Hardy Ubuntu</a>&#8220;
  * Fedora forum topic &#8220;[Network controller: Broadcom Corporation BCM4328 802.11a/b/g/n](http://forums.fedoraforum.org/showthread.php?t=182684)&#8220;
  * Ubuntu bug #214917 &#8220;<a href="https://bugs.launchpad.net/ubuntu/+source/linux/+bug/214917" target="_blank">Module b44 interfering with ndiswrapper upon startup</a>&#8220;
  * Ubuntu bug #197558 &#8220;<a href="https://bugs.launchpad.net/ubuntu/+source/linux/+bug/197558" target="_blank">ssb module breaks BCM4328 with ndiswrapper (regression from 2.6.24-10)</a>&#8220;