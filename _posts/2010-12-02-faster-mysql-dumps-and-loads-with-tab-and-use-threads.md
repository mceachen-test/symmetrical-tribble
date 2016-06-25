---
ID: 1047
post_title: 'Faster MySQL dumps and loads with &#8211;tab and &#8211;use-threads'
author: matthew
post_date: 2010-12-02 12:26:15
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/faster-mysql-dumps-and-loads-with-tab-and-use-threads-1047.html
published: true
dsq_thread_id:
  - "183613577"
---
By default, mysqldump writes a series of sql DDL and inserts to standard out, that you can then pipe to another database server to recreate a given database.

The problem is that this is all serial, and if you're having to do this task regularly (because you're sharing databases between different development environments, for example), it'd be nice if this could be sped up, and it certainly can with the current 5.1.x versions of MySQL.

<!--more-->

MySQL recently added two new features that help with this: <tt>mysqldump --tab=path</tt> and <tt>mysqlimport --use-threads=N</tt>.

<h3>Dumping</h3>

Here's how you'd dump multiple databases without --tab (assuming your ~/.my.cnf told mysqldump how to connect to your database server):

<pre lang="bash">mysqldump --opt --all-databases | gzip > dump.sql.gz</pre>

There are a bunch of caveats to using the <tt>--tab</tt> option:
<ul>
<li>The mysqldump command <strong>must be run on the database server</strong>, because mysqldump will invoke SELECT INTO OUTFILE on the server.
<li> The FILE permission must be granted to the mysqldump user
<li> The directory needs to be writeable by the mysqld euid
<li> If you want to dump multiple databases, you'll need to create a new directory per database so same-named tables won't clobber eachother
</ul>  

<em>In the interests of simplicity, I used globally-read-write permissions here. If untrusted users had access to these directories, these permissions would be unacceptable, of course.</em>

<pre lang="bash">
dir=$(date "+%Y-%m-%d_%Hh%Mm")
mkdir -m 777 -p /tmp/$dir
for db in $(mysql -BNe "show databases" | grep -v information_schema ) ; do 
  mkdir -m 777 /tmp/$dir/$db
  mysqldump --tab=/tmp/$dir/$db --opt --single-transaction --quick $db
done
</pre>

<h3> Loading </h3>

Loading from a .sql.gz file is trivial -- just pipe it to mysql and call it a day:

<pre lang="bash">gzcat dump.sql.gz | mysql</pre>

Loading from tab files is a bit more work. <b>Note that this NUKES AND PAVES your database with the content of the dump--including the mysql users, their passwords, and their permissions!</b> You'll also want to play with --use-threads, depending on the number of processors your machine hardware has.

<pre lang="bash">
cd ${DUMP_DIR} # <- where DUMP_DIR is the database dump you want to load, like /tmp/$dir from above
for db in * ; do
  mysql -e "drop database $db; create database $db default charset utf8"
  cat $db/*.sql | mysql $db
  mysqlimport --use-threads=3 --local $db $db/*.txt
done
</pre>