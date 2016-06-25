---
ID: 400
post_title: >
  Installing CrashPlan on Ubuntu 9.04
  Server Edition
author: matthew
post_date: 2009-05-19 22:00:28
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/installing-crashplan-on-ubuntu-904-server-edition-400.html
published: true
syntaxhighlighter_encoded:
  - "1"
dsq_thread_id:
  - "161139824"
---
Although <a href="http://www.rsnapshot.org/">rsnapshot</a> is super for linux-to-linux backups, I've found <a href="http://www.crashplan.com">CrashPlan</a> to work very well as a backup solution for my family's windows and mac boxes.

The CrashPlan installation works pretty well on ubuntu desktop edition, as all the necessary packages are already there. The server edition fails quietly, though.

Run these steps before you install CrashPlan, and everything should be smooth:
<!--more-->
1. First get all the prerequisite packages (including Sun's JRE and SWT):

<pre class="lang:bash decode:1 " >
sudo apt-get install sun-java6-jre sun-java6-bin sun-java6-fonts libswt-gtk-3.4-java xauth
</pre>

2. Tell Ubuntu to use the sun-java6 JRE by default:

<pre class="lang:bash decode:1 " >
sudo update-java-alternatives -s java-6-sun
</pre>

And then know that the log directory (if you accept the default installation directories) will be in <tt>/usr/local/crashplan/log</tt>.

3. Once installed, you can see the CrashPlan UI by using ssh X11-forwarding. On a Mac, install <a href="http://xquartz.macosforge.org/trac/wiki/X112.4.0">X11</a>, then run <tt>ssh -X ubuntu-server</tt> (where "ubuntu-server" is the hostname of the server running CrashPlan). The -X tells ssh to forward X11 traffic. If you see this error:
<pre>$ ssh -X my-server
Warning: untrusted X11 forwarding setup failed: xauth key data not generated
Warning: No xauth data; using fake authentication data for X11 forwarding.</pre>
then you need to install the "xauth" package on the ubuntu server:

<pre class="lang:bash decode:1 " >sudo apt-get install xauth</pre>

If you see "<tt>connect /tmp/.X11-unix/X0: No such file or directory</tt>" you need to start X11.app on you Mac before running ssh -X.

<h3>Update December 3, 2009:</h3>
I upgraded to Ubuntu 9.10 with <tt>do-release-upgrade</tt> and crashplan looks like it's compatible with Karmic Koala.

<h3>Update December 15, 2009:</h3>
I upgraded my Mac to Snow Leopard, which installs a broken X11--the text entry fields hang after a couple keypresses, and the buttons don't click with mouse. As per the <a href="http://xquartz.macosforge.org/trac/wiki/Releases">XQuartz releases page</a>, I'll try <code>sudo port -v install xorg-server</code> and see how that works.