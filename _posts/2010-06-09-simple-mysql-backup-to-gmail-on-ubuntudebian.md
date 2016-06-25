---
ID: 882
post_title: >
  Simple MySQL backup to gmail on
  Ubuntu/Debian
author: matthew
post_date: 2010-06-09 18:38:47
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/simple-mysql-backup-to-gmail-on-ubuntudebian-882.html
published: true
dsq_thread_id:
  - "161339120"
---
Backing up your MySQL database (if it's a reasonable size, like &lt; 100s of MB) can be done with a cronjob that runs mysqldump, gzip, and mpack.

<!--more-->

First set up your server to use gmail as the Mail Transfer Agent:

<pre lang="bash">$ sudo apt-get install ssmtp mpack</pre>

Then edit <code>/etc/ssmtp/ssmtp.conf</code>, replacing the YOUR... values as appropriate:
<pre lang="bash"># Where will the mail seem to come from?
rewriteDomain=YOURDOMAIN.com
# The full hostname of localhost:
hostname=HOSTNAME
root=root@YOURDOMAIN.com
AuthUser=YOURGMAILUSER
AuthPass=YOURGMAILPASSWORD
mailhub=smtp.gmail.com:587
UseSTARTTLS=YES
FromLineOverride=YES</pre>

Then copy the following into a new file called <code>/etc/cron.daily/nightly-backup</code>. Note that this is using the MySQL username and password that the Debian/Ubuntu package has installed. If you installed MySQL from source, you'll want to change the defaults-file location.

<pre lang="bash">
#!/bin/sh
mkdir -p /backup/mysqldump/
mysqldump --defaults-file=/etc/mysql/debian.cnf --all-databases | gzip > /backup/mysqldump/$(date '+%Y%m%d').sql.gz
mpack -s "mysql backup" /backup/mysqldump/$(date '+%Y%m%d').sql.gz "YOURGMAILUSER"
</pre>