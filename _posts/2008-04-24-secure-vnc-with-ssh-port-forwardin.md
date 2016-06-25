---
ID: 42
post_title: Secure VNC with ssh port forwarding
author: matthew
post_date: 2008-04-24 13:13:38
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/secure-vnc-with-ssh-port-forwardin-42.html
published: true
btc_comment_summary:
  - 'a:0:{}'
btc_comment_counts:
  - 'a:0:{}'
dsq_thread_id:
  - "160993270"
---
Need to help out a damsel in distress (username "damsel") sitting on a remote debian/ubuntu box ("remotehost")? Have you <a href="http://www.karlrunge.com/x11vnc/#firewalls">set up ssh on a non-standard port</a> (port 12345) already? Great. Keep reading.

Step 1: Install <a href="http://www.karlrunge.com/x11vnc/">x11vnc</a> on the remote machine:

<pre>
ssh remotehost
sudo apt-get install x11vnc
</pre>

Step 2: Spin up x11vnc on the remote host:

<pre>
ssh remotehost 
sudo -u damsel x11vnc -noxdamage -speeds dsl -solid -display :0 -passwd SECRET
</pre>
Keep this ssh running. <tt>-speeds dsl -solid</tt> makes vnc more responsive.

Step 3: Forward the remotehost's vnc port, 5900, to your local host using ssh:

<pre>
ssh -p 12345 -L 5900:127.0.0.1:5900 remotehost
</pre>

Step 4: Start up your VNC viewer application, pointing to localhost. 

<a href="http://www.tightvnc.com/">Tightvnc</a> is a great vncviewer and is installable through macports:

<pre>
sudo port install tightvnc
</pre>

Then run:

<pre>
vncviewer localhost
</pre>