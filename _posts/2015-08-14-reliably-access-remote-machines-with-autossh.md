---
ID: 1424
post_title: >
  Reliably access remote machines with
  autossh
author: matthew
post_date: 2015-08-14 00:19:15
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/reliably-access-remote-machines-with-autossh-1424.html
published: true
dsq_thread_id:
  - "4031501895"
---
If you have a machine behind a NAT that you don't want to directly expose to the internet, but want access to it, you can use <a href="http://www.harding.motd.ca/autossh/">autossh</a> to create a tunnel.

Assuming the hostname of the machine behind the NAT is <code>pi</code>, and the host you can access is <code>server</code>, do the following steps:

<!--more-->

<ol>
<li>On both machines, <strong>create a non-root account</strong>: <pre>sudo useradd autossh</pre></li>

<li>On <tt>pi</tt>, make sure <strong>both <code>sshd</code> and <code>autossh</code> are installed</strong>. For Debian/Ubuntu:

<pre>sudo apt-get install openssh-server openssh-client autossh</pre></li>

<li>On <tt>pi</tt>, as user <tt>autossh</tt>, generate a strong ssh key pair:<pre>ssh-keygen -b 4096</pre></li>

<li>On <tt>server</tt>, as user <tt>autossh</tt>, add the contents of <tt>pi</tt>'s <tt>~/.ssh/id_rsa.pub</tt> into <tt>~/.ssh/authorized_keys</tt>, and <tt>chmod 600 .ssh/authorized_keys</tt></li>

<li>On <tt>pi</tt>, as user <tt>autossh</tt>, <tt>ssh server</tt> (use the correct hostname, of course). You should not have to type a password to log in.</li>

<li>On <tt>pi</tt>, add the following line to <tt>/etc/rc.local</tt>, above the <tt>exit 0</tt>: 
<pre>
sleep 30 # hack to wait for networking to start
su --login --command "autossh -- -N -R 2222:localhost:22 server" autossh &
</pre></li>

<li>Reboot <tt>pi</tt>. Wait a minute for the tunnel to start.</li>

<li>You should be able to log into <tt>pi</tt> from <tt>server</tt> now: <pre>ssh localhost -p 2222</pre> </li>
</ol>