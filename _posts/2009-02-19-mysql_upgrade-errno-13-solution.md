---
ID: 324
post_title: 'Solution for mysql_upgrade errno: 13'
author: matthew
post_date: 2009-02-19 15:40:48
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/mysql_upgrade-errno-13-solution-324.html
published: true
syntaxhighlighter_encoded:
  - "1"
dsq_thread_id:
  - "161296603"
---
Are you upgrading MySQL, and seeing this?

<pre>
$ mysql_upgrade 
Looking for 'mysql' as: mysql
Looking for 'mysqlcheck' as: mysqlcheck
Running 'mysqlcheck'...
....
Could not create the upgrade info file
   '/usr/local/mysql/data/mysql_upgrade_info' 
in the MySQL Servers datadir, errno: 13
</pre>

Solution -- <strong>run the script as root</strong> (so you'll have permission to write to mysql_upgrade_info).