---
ID: 773
post_title: >
  How to fix the wifi on a HP Mini 110
  Netbook running Ubuntu 9.10 (Karmic
  Koala)
author: matthew
post_date: 2010-02-01 23:19:40
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/how-to-fix-the-wifi-on-a-hp-mini-110-netbook-running-ubuntu-9-10-karmic-koala-773.html
published: true
syntaxhighlighter_encoded:
  - "1"
dsq_thread_id:
  - "161982949"
---
Installing Karmic Koala from a USB drive proved to be a bit less than effortless. First off the http and ftp servers for the netbook-reloaded ISO images were really slow, and for some odd reason there aren't any bittorrent links for the netbook-reloaded image. You need to go to <a href="http://releases.ubuntu.com/9.10/">http://releases.ubuntu.com/9.10/</a> to find the .torrent link (which is <a href="http://releases.ubuntu.com/9.10/ubuntu-9.10-netbook-remix-i386.iso.torrent">here</a>).

The <a href="https://help.ubuntu.com/community/Installation/FromUSBStick">installation instructions</a> need some editing, too, but the jist is if you've got a linux box, use usb-creator -- and don't panic when the usb-creator main window disappears (and doesn't get replaced with a progress window for several minutes).

So the main glitch was wireless -- it didn't work out of the box on my HP 110 netbook.

<!--more-->

There's instructions to install some random linux source package, but that's a red herring -- just go find a wired network connection, run

<pre class="lang:bash decode:1 " >sudo apt-get update
sudo apt-get dist-upgrade</pre>

which will then prompt for a reboot. Wait for the reboot (it's fast! just a couple seconds to a working X!), then go to System &gt; Hardware Drivers, and it'll find the drivers you need automatically. Click them to enable, wait for them to install, reboot, and wireless should work.