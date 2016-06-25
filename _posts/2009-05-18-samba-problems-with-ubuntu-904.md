---
ID: 397
post_title: Samba problems with Ubuntu 9.04
author: matthew
post_date: 2009-05-18 22:25:40
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/samba-problems-with-ubuntu-904-397.html
published: true
syntaxhighlighter_encoded:
  - "1"
dsq_thread_id:
  - "160993454"
---
I was trying to set up samba with a new Ubuntu 9.04 ("Jaunty Jackalope") box, and was frustrated when windows failed to connect to the [homes] share I had enabled.

It turns out that even if you comment out all the printer configuration in /etc/samba/smb.conf, smbd will still try to connect to CUPS, and quietly die.

After setting smb.conf to log to syslog, I saw:
<pre wrap="">
smbd[30539]: printing/print_cups.c:cups_connect(103)
smbd[30539]:   Unable to connect to CUPS server localhost:631 - Connection refused
</pre>

The solution? Add this to the <code>[global]</code> section:

<pre>load printers = no</pre>

and pick up the new setting by running 
<pre>/etc/init.d/samba restart</pre>