---
title: "Transferring Files from iOS to Linux wirelessly"
description: "Transfer files wirelessly between iOS devices and a Linux computer using ssh."
tags: ["ios", "linux", "ssh", "fedora"]
date: 2022-08-06T09:29:40+09:30
---

## Overview
I was an Android user for a long period of time, right from the Jellybean and 
Gingerbread days. After almost a decade being with Android I finally moved to
iOS, because I was sick of the fragmentation and I wish Android took privacy 
as seriously as iOS does. One of the most common things I need to do is transfer
files across from my iOS device to my Linux laptop. One way of doing this is to 
connect my laptop and iPhone with a lightning cable, however it is super
inconvenient and you don't always have a cable lying around. You can email 
the file to yourself but thats a really roundabout way to do it. 

With an Android device, things are easier as Google allows apps on the playstore
that have built inservers. iOS does not allow such apps. However, what iOS does
support is being able to connect to other servers like an ssh server.

If you have a Macbook and an iOS device, a wireless transfer is very easy - 
Airdrop. However, with Linux, you're out of luck. The only reliable way to
transfer files to a Linux laptop is via sftp or ftp over ssh.

## Install ssh server on Linux
In this part of the post, I'll show you how to ==install== an SSH server on Fedora Linux. 

First, we'll need to install the openssh-server package:
```bash
sudo dnf install openssh-server
```

Once the package is installed, we'll need to start the SSH service:
```bash
sudo systemctl enable sshd # ensure sshd starts up after reboot
sudo systemctl start sshd # starts the ssh daemon
sudo systemctl status sshd # check status of service
```

After you have installed the ssh server on your laptop move on to the next part
of this post. You will need your iOS device - iPhone or iPad for this.

## Install FE File Explorer on your iOS device
FE File Explorer is an app which lets you connect to servers from your iOS device.
It can connect to a variety of sources including NAS, FTP, SAMBA etc.

It is a free application that also has a paid version but even the free version
has a lot of built in functionality and it will suit the needs of most users.

Once you launch the app, hit the + button on the top right hand corner and the 
following screen will be shown -

![FE File Explorer New Window](/images/fexplorer/fexplorer_add.jpg "FE File Explorer New Window")
<p class="subtitle">FE File Explorer New Window</p>

Select 'SFTP'. If 'SFTP' is not available you should be able to select 'FTP' although
I have not tested it with this option.

Enter details in the next screen. The 'Hostname' (IP Address), 'Username' and 'Password'
are the most important fields. You can check the IP address of the target computer
using the following command -
```bash
ip a
```

The username and password are the account details you use on your Linux computer.

If you followed the instructions correctly, you should now see an entry for your
connection at the main screen of the app. Once you tap it, you should be able
to view all the files and folders of your Linux computer.

![FE File Explorer Connection Added](/images/fexplorer/fexplorer_main.jpg "FE File Explorer Connection Added")
<p class="subtitle">FE File Explorer Connection Added</p>

Transferring files is easy, you can simply hit the 3 dots for a File context menu 
as shown below -

![FE File Explorer Popup Menu](/images/fexplorer/fexplorer_filepopup.jpg "FE File Explorer Popup Menu")
<p class="subtitle">FE File Explorer Popup Menu</p>

This seems like a lot of steps but once you have set things up its like using
any other app on your phone.
