---
ID: 925
post_title: 'HOWTO: Configure an Amazon RDS instance to use UTF-8'
author: matthew
post_date: 2010-08-15 11:22:06
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/howto-configure-an-amazon-rds-instance-to-use-utf-8-925.html
published: true
dsq_thread_id:
  - "161131565"
---
RDS has been working out pretty well for <a href="http://adgrok.com">AdGrok</a> -- it's a one-click MySQL 5.1 instance that seems pretty promising:
<ul>
	<li>Automatic replication and failover to another deployment zone (colo)</li>
	<li>Easy to set up firewall configuration</li>
	<li>Easy to scale up (but not down) for CPU and disk space</li>
</ul>
If you're just starting out, it's a whole lot simpler than, say, a DRBD/Heartbeat/MySQL configuration running on EC2 instances.

There's just one catch. It doesn't use UTF-8 encoding by default, it uses latin1_swedish. If you're going to do business outside the US, utf8 is a must.

The second catch -- the Amazon web UI for managing RDS "parameter groups" is read-only. If you know the magick incantations, though, it's not that bad. Here's how to make it all go:

<!--more-->

<h3>Install the RDS tools</h3>

I found them <a href="http://developer.amazonwebservices.com/connect/entry.jspa?categoryID=294&amp;externalID=2928">here</a>, but that URL doesn't seem very "authoritative." The googles might find a newer version for you.

Unzip the archive into <code>/opt</code>, and add this to your <code>~/.bashrc</code> -- but note that the JAVA_HOME is a mac-specific thing -- change that, and the REGION, to your liking, and go fetch your specific cert and private key from your EC2 "account" page. Note also that this assumes you made a symlink from the version of the RDS tools to <code>/opt/rds</code>, and from the ec2 tools to <code>/opt/ec2</code>.

<pre lang="bash">
export JAVA_HOME=$(/usr/libexec/java_home)
export EC2_HOME=/opt/ec2
export EC2_REGION=us-west-1
export AWS_RDS_HOME=/opt/rds
export EC2_PRIVATE_KEY=$HOME/.ssh/pk-SOMETHINGSOMETHING.pem
export EC2_CERT=$HOME/.ssh/cert-SOMETHINGSOMETHING.pem
export PATH=$EC2_HOME/bin:$AWS_RDS_HOME/bin:$PATH
</pre>

<h3>Create your RDS instance</h3>

OK. Let's assume you've got all that squared away now, and that you're starting out with a new database (so you don't have to <a href="http://snippets.dzone.com/posts/show/6070">convert the contents from latin1 to utf8</a>). In the web interface, click the RDS tab, and create a new RDS instance. If in doubt, create a new security group. You can change the definition of the security group, but you can't change the group for a given RDS instance. I set up "classes" of security groups for AdGrok -- one for the blog, and one for the production application. 

Let's also assume you named your database "db1" because you're an engineer, and think that creativity in naming should be reserved for yachts.

<h3>Set up and assign the Parameter Group</h3>

Create and configure a new "parameter group" -- the default parameter group, <code>default.mysql5.1</code>, isn't editable.

<pre lang="bash">
rds-create-db-parameter-group utf8 -e mysql5.1 -d utf8 

rds-modify-db-parameter-group utf8 \
  --parameters="name=character_set_server, value=utf8, method=immediate" \
  --parameters="name=collation_server, value=utf8_general_ci, method=immediate" 
</pre>

Then assign your database instance to use the new group:

<pre lang="bash">
rds-modify-db-instance db1 --db-parameter-group-name utf8 
</pre>

And the final bit of pain -- to make the parameter group settings take affect, you need to reboot the instance. Note that this will take several minutes before you see your RDS instance back online.

<pre lang="bash">
rds-reboot-db-instance db1 
</pre>

And finally, test that everything stuck by opening a mysql prompt:

<pre lang="sql">
mysql> show variables like '%character%';
+--------------------------+-------------------------------------------------+
| Variable_name            | Value                                           |
+--------------------------+-------------------------------------------------+
| character_set_client     | utf8                                            |
| character_set_connection | utf8                                            |
| character_set_database   | utf8                                            |
| character_set_filesystem | binary                                          |
| character_set_results    | utf8                                            |
| character_set_server     | utf8                                            |
| character_set_system     | utf8                                            |
</pre>

You want all utf8s -- if you don't, make sure your .my.cnf has these lines:
<pre lang="bash">
[client]
host=....rds.amazonaws.com
default-character-set=utf8
</pre>
and, if you're on Ruby on Rails, your config/database.yml has these lines:
<pre lang="yaml">
production:
  adapter: mysql
  encoding: utf8
  collation: utf8_general_ci
</pre>