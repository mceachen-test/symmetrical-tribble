---
ID: 944
post_title: >
  Simple 301 or 302 redirects with Apache
  or PHP
author: matthew
post_date: 2010-09-03 12:54:50
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/simple-301-or-302-redirects-with-apache-php-944.html
published: true
dsq_thread_id:
  - "162407387"
---
It behooves you to make sure you've only got one URL that serves a given piece of content--<a href="http://www.google.com/support/webmasters/bin/answer.py?hl=en&answer=93633">so says google</a>. But what if you've got a bunch of domains that go to your page? 

For this blog, for example, <a href="http://matthew.mceachen.org">matthew.mceachen.org</a>, and <a href="http://matthew.mceachen.us">mrm.mceachen.org</a> all go to the same place. That was done with an apache redirect in <code>/etc/apache2/sites-enabled/matthew</code>:

<!--more-->

<pre lang="xml">
<VirtualHost *:80>
    ServerName matthew.mceachen.org
    ServerAlias www.matthew.mceachen.us mrm.mceachen.org
    Redirect permanent / http://matthew.mceachen.us/
</VirtualHost>
</pre>

If you want to redirect from one directory to another, PHP makes that pretty simple. This is the contents of my <code>~/public_html/index.php</code> that directs people into my blog:

<pre lang="php">
<?php
header('Location: http://matthew.mceachen.us/blog/');
?>
</pre>

You can <a href="http://en.wikipedia.org/wiki/301_redirect">read more about 301 redirects on wikipedia</a>.