---
ID: 876
post_title: >
  HOWTO enable the query log on MySQL on
  Mac OS X
author: matthew
post_date: 2010-05-27 20:38:56
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/howto-enable-the-query-log-on-mysql-on-mac-os-x-876.html
published: true
dsq_thread_id:
  - "161337669"
---
Tailing the MySQL query log in real time can be a lifesaver for any developer, and it's pretty easy to do.

<em>(Updated 20150905 for MySQL 5.6.x)</em>

<h2>2. Tell MySQL to write to the logfile</h2>

If you've installed MySQL 5.6.x via Homebrew, you won't have an <code>/etc/my.cnf</code>, but it just needs to have these two lines:
<pre lang="bash">[mysqld]
general_log=1
general_log_file=/usr/local/var/mysql/mysql-query.log
</pre>

If you've installed MySQL via the .pkg, the path to the logfile will be different.

See <a href="https://dev.mysql.com/doc/refman/5.6/en/query-log.html">the fine manual</a> for more details.

<h2>3. Restart mysqld</h2>

If you have the MySQL preference pane, open that, click stop, then start. (It turns out that when the preference pane is open, it pings the database every 2 seconds, so it can detect if the db is alive. If you mangle the my.cnf, you'll find the start button seems to not respond to clicks.)

If you installed MySQL with homebrew or MacPorts, and installed a launchd script to start MySQL on startup, you can run <code>mysqladmin shutdown</code> and launchd will restart MySQL for you.

<h2>4. Finally: watch the query log</h2>

Run <code>tail -f /usr/local/var/mysql/mysql-query.log</code>