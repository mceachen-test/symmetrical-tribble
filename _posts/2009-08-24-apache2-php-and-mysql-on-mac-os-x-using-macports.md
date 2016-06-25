---
ID: 573
post_title: >
  Apache2, PHP, and MySQL on Mac OS X
  using MacPorts
author: matthew
post_date: 2009-08-24 22:46:59
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/apache2-php-and-mysql-on-mac-os-x-using-macports-573.html
published: true
syntaxhighlighter_encoded:
  - "1"
dsq_thread_id:
  - "162093744"
---
<h3>1. Install MacPorts</h3>
Follow the instructions here: <a href="http://www.macports.org/install.php">http://www.macports.org/install.php</a>.
<h3>2. Install apache2</h3>
<pre>sudo port install apache2</pre>
Note that the macports instructions suggest installing the launchctl script now, but we'll do that after mysql and php are installed.
<h3>3. Install and configure MySQL</h3>
If you want 5.0.x, use <code>mysql5-server</code>. If you need 5.1.x, install <code>mysql5-server-devel</code> (at least as of August 2009).
<pre>sudo port install mysql5-server</pre>
As the macports instructions state, 
<blockquote>In order to setup the database, you might want to run <tt>sudo -u mysql mysql_install_db5</tt> if this is a new install.</blockquote>
It's never a bad idea to set the root password, and as the document suggests, run:
<pre>/opt/local/lib/mysql5/bin/mysqladmin -u root password 'new-password'</pre>
You also want to install a database configuration file -- there are a bunch of templates in /opt/local/share/mysql5/mysql/, but for development, my-small.cnf should suffice:
<pre>sudo cp /opt/local/share/mysql5/mysql/my-small.cnf /opt/local/etc/mysql5/my.cnf</pre>
Once the config is in place, spin up mysql:
<pre>sudo launchctl load -w /Library/LaunchDaemons/org.macports.mysql5.plist</pre>
Check that mysql is up and running by connecting with the mysql client:
<pre>mysql -h localhost -u root -p</pre>
<h3>4. Fix your PATH</h3>
Note that the mysql binaries in /opt/local/bin all have a "5" suffix, but /opt/local/lib/mysql5/bin has "normal" named binaries, so you probably want that in your PATH too. The <code>apachectl</code> in /usr/bin will spin up the mac os x version of apache (that we're avoiding), and that lives in /opt/local/apache2/bin. So in your .bashrc (or .profile or whatever):
<pre>export PATH=/opt/local/bin:/opt/local/sbin:<strong>/opt/local/lib/mysql5/bin:/opt/local/apache2/bin</strong>:$PATH</pre>
<h3>5. Install PHP5</h3>
<pre>sudo port install php5 +pear +apache2 +fastcgi +mysql5</pre>
Note that php5 has a lot of variants. If you think you want other goodness, run <code>port variants php5</code> and cook up your own set of options.

Again, as the macports instructions state,
<blockquote>copy <code>/opt/local/etc/php5/php.ini-development </code>(if this is a development server) or <code>/opt/local/etc/php5/php.ini-production</code> (if this is a production server) to <code>/opt/local/etc/php5/php.ini</code> and then make changes.</blockquote>

<h3>6. Install the PHP-MySQL driver:</h3>
<pre>sudo port install php5-mysql +mysql5</pre>

<h3>7. Configure apache2</h3>
The <code>mod_php.conf</code> from the php5 package is put into a directory that the apache2 configuration doesn't read by default -- so you need to add this line to the end of <code>/opt/local/apache2/conf/httpd.conf</code>:
<pre>Include conf/extras-conf/*</pre>
Hopefully this will be considered a packaging bug, and will be fixed at some point.

<h3>8. Run apache2</h3>
<pre>sudo launchctl load -w /Library/LaunchDaemons/org.macports.apache2.plist</pre>
Note that the logs, by default, are in <code>/opt/local/apache2/logs</code>.

If you change the PHP or Apache configuration files, run <pre>sudo /opt/local/apache2/bin/apachectl restart</pre> and watch the logs for errors.