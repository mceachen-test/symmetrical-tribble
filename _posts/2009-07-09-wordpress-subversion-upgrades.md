---
ID: 45
post_title: WordPress upgrade with svn sw
author: matthew
post_date: 2009-07-09 20:52:49
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/wordpress-subversion-upgrades-45.html
published: true
syntaxhighlighter_encoded:
  - "1"
dsq_thread_id:
  - "160993284"
---
Most likely, it's time to upgrade your instance of wordpress (given their releases are every couple weeks). The new built-in automatic upgrade works if you make your blog owned by the apache user, but if you have any local changes, they get blown away by the upgrade. If you use subversion, your changes stay AND you get the security patches. To install via subversion, first <a href="http://codex.wordpress.org/Installing/Updating_WordPress_with_Subversion">follow these steps</a>.

Before you upgrade, always backup your database and blog code:

<pre lang="bash">
mkdir -p ~/.archive
mysqldump --opt blog | gzip > ~/.archive/blog-$(date '+%Y%m%d').sql.gz
cd ~/public_html 
tar czf ~/.archive/blog-$(date '+%Y%m%d').tgz blog
</pre>

Then upgrade. <strong>Note that switching from 2.7.1 to 2.8 isn't just a "svn switch" -- you must <code>svn update</code> first.</strong>

<pre>cd ~/public_html/blog
svn update
svn sw http://svn.automattic.com/wordpress/tags/3.0.3/</pre>

The next time you hit the wp-admin page, it will upgrade the db and you'll be done.

Note that if you see
<pre>svn: 'http://core.svn.wordpress.org/tags/2.8.6'
is not the same repository as
'http://svn.automattic.com/wordpress'</pre>
you should use this svn sw command instead:
<pre>svn sw http://core.svn.wordpress.org/tags/3.0.3/</pre>
<hr />
	<li> Update November 15, 2008: 2.6.5 is out. See <a href="http://codex.wordpress.org/Installing/Updating_WordPress_with_Subversion#Updating_to_a_New_Stable_Version">installing wordpress using subversion</a>.</li>
	<li> Update December 14, 2008: 2.7 with new admin UI hotness</li>
	<li> Update June 15, 2009: 2.8 requires an `svn update`</li>
	<li> Update July 9th, 2009: <a href="http://wordpress.org/development/2009/07/wordpress-2-8-1/">2.8.1</a></li>
	<li> Update July 22nd, 2009: oye, <a href="http://wordpress.org/development/2009/07/wordpress-2-8-2/">2.8.2</a></li>
	<li> Update Aug 4th, 2009: oye, <a href="http://wordpress.org/development/2009/08/wordpress-2-8-3-security-release/">2.8.3</a></li>
	<li> Update Aug 17, 2009: <a href="http://wordpress.org/development/2009/08/2-8-4-security-release/">2.8.4</a></li>
	<li>Update Oct 20, 2009: <a href="http://wordpress.org/development/2009/10/wordpress-2-8-5-hardening-release/">2.8.5</a></li>
	<li>Update Nov 15, 2009: <a href="http://wordpress.org/development/2009/11/wordpress-2-8-6-security-release/">2.8.6</a></li>
	<li>Update Jan 2010: 2.9.1</li>
	<li>Update March 2010: 2.9.2</li>
	<li>Update July 2010: 3.0</li>
        <li>Update Fall 2010: I give up. It's easier to just use the wordpress upgrade system, even though it deletes your changes.<li>