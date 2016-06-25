---
ID: 679
post_title: OpenDNS updater for linux/ubuntu
author: matthew
post_date: 2009-11-15 11:51:43
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/opendns-updater-for-linux-ubuntu-679.html
published: true
syntaxhighlighter_encoded:
  - "1"
dsq_thread_id:
  - "160993497"
---
The <a href="http://www.opendns.com/">OpenDNS</a> service is great -- it provides anti-phishing and the ability to filter out some of the less desirable detritus from the internets.

OpenDNS needs to be periodically notified about what your IP address is, and I don't have a windows or macintosh box that's always on. I do have an ubuntu box, though, but there weren't any instructions on OpenDNS' site to do this properly.

<!--more-->

Cron does periodic jobs very well -- but rather than using "crontab -e", it's much better to install a system-level cron job by adding a file to the <code>/etc/cron.d</code> directory. The file only needs to be readable -- there's no need to set the execute bit. You can choose the effective user that will run the command (in this case I ran as "nobody"), and a backup of your system that includes /etc will pick up your crontab entry.

Here's the contents of <code>/etc/cron.d/opendns</code>:

<pre class="lang:bash decode:1 " >
47 * * * * nobody curl -u USERNAME:PASSWORD -s https://updates.opendns.com/nic/update | grep -vE &quot;^good&quot;
</pre>

Replace USERNAME and PASSWORD with your opendns username and password. 

The grep squelches success messages. No news is good news.