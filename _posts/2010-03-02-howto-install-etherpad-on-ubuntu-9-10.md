---
ID: 811
post_title: HOWTO install etherpad on ubuntu 9.10
author: matthew
post_date: 2010-03-02 00:14:17
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/howto-install-etherpad-on-ubuntu-9-10-811.html
published: true
syntaxhighlighter_encoded:
  - "1"
dsq_thread_id:
  - "160993546"
---
<a href="http://etherpad.com/">Etherpad</a> was <a href="http://etherpad.com/ep/blog/posts/etherpad-back-online-until-open-sourced">opensourced by google</a>, and has some <a href="http://code.google.com/p/etherpad/wiki/Instructions">generic installation instructions.</a> Here's the translation for Ubuntu Karmic Koala (release 9.10):

<!--more-->
<h3>Install the prerequisites:</h3>
<pre class="lang:bash decode:1 " >
sudo apt-get install mysql-server-5.1 mercurial sun-java6-jdk sun-java6-jre sun-java6-bin
cd /tmp
wget http://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.1.12.tar.gz/from/http://mysql.he.net/
wget http://www.scala-lang.org/downloads/distrib/files/scala-2.7.7.final.tgz
sudo mkdir -p /opt
cd /opt
sudo tar xvzf /tmp/mysql*tar.gz
sudo tar xvzf /tmp/scala*.tgz
sudo ln -s scala* scala
</pre>
<h3>Create an etherpad system user:</h3>
<pre class="lang:bash decode:1 " >
sudo adduser --system etherpad
sudo -u etherpad bash
</pre>

Add this to <code>~etherpad/.bashrc</code>:

<pre class="lang:bash decode:1 " >
export JAVA_HOME=/usr/lib/jvm/java-6-sun
export JAVA=&quot;$JAVA_HOME/bin/java&quot;
export SCALA_HOME=/opt/scala
export SCALA=$SCALA_HOME/bin/scala
export PATH=&quot;$JAVA_HOME/bin:$SCALA_HOME/bin:$PATH&quot;
export MYSQL_CONNECTOR_JAR=&quot;/opt/mysql-connector-java-5.1.12/mysql-connector-java-5.1.12-bin.jar&quot;
</pre>
<h3>Check out the source and install the mysql database:</h3>
<pre class="lang:bash decode:1 " >
cd ~etherpad
hg clone https://etherpad.googlecode.com/hg/ etherpad
mysql -u root -p -e &quot;create database etherpad&quot;
mysql -u root -p -e &quot;grant all privileges on etherpad.* to 'etherpad'@'localhost' identified by 'password'; flush privileges;&quot;
cd ~etherpad/etherpad/trunk/etherpad
bin/rebuildjar.sh
</pre>

Compile:

<pre class="lang:bash decode:1 " >
cd ~etherpad/etherpad/trunk/etherpad
bin/rebuildjar.sh
</pre>
<h3>Configure the domain of your server</h3>
<ul>
	<li>Add the domain name of your server to the SUPERDOMAIN config in <code>~etherpad/etherpad/trunk/etherpad/src/etherpad/globals.js</code></li>
	<li>Remember to add port forwarding for port 9000 on your router.</li>
</ul>
<h3>Start Etherpad on @reboot with cron:</h3>
Run <code>sudo crontab -u etherpad -e</code> and paste this in:
<pre class="lang:bash decode:1 " >
MAILTO=root
# m h  dom mon dow   command
@reboot cd ~etherpad/etherpad/trunk/etherpad ; bin/run-local.sh
</pre>

<h4>Update May 10, 2010</h4>
I just found <a href="http://pauleira.com/13/installing-etherpad/">Nuba Princigalli</a>'s post which has etherpad pro instructions as well.