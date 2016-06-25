---
ID: 52
post_title: >
  Preventing an external hard drive from
  idling on ubuntu
author: matthew
post_date: 2008-06-09 05:07:35
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/preventing-an-external-hard-drive-from-idling-on-ubuntu-52.html
published: true
btc_comment_counts:
  - 'a:0:{}'
btc_comment_summary:
  - 'a:0:{}'
dsq_thread_id:
  - "166146122"
---
I got a <a href="http://www.seagate.com/www/en-us/products/external/freeagent_pro_data_movers/">Seagate FreeAgent Pro</a> external hard drive for backups (JWZ has a <a href="http://www.jwz.org/doc/backups.html">very straightforward article about this</a>). It happily reformatted to ext3, and I kicked off an rsync of /home.

Because rsync figures out what files need copying before it copies them, and there are hundreds of thousands of files in my /home, there was more than a couple minutes of grinding on the local hard drive building a list of files to copy over. While this happened, the external drive idled into a "sleep" mode that ubuntu can't seem to awaken it from.

This was <a href="http://alienghic.livejournal.com/382903.html">slashdotted</a> with an sdparm hack, but I believe <a href="http://www.nslu2-linux.org/wiki/FAQ/DealWithAutoSpinDownOnSeagateFreeAgent">this solution</a> is better. Copy this new udev rule into <code>/etc/udev/rules.d/50-local.rules</code> (this is a new file that you will be creating):
<pre wrap=1># Seagate FreeAgent allow_restart fix (i/o errors)
SUBSYSTEMS=="scsi",DRIVERS=="sd",ATTRS{vendor}=="Seagate*",ATTRS{model}=="FreeAgent*",RUN+="/bin/sh -c 'echo 1 &gt; /sys/class/scsi_disk/%k/allow_restart'"
</pre>