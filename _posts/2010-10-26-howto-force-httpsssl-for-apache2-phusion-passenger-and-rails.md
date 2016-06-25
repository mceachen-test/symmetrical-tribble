---
ID: 1005
post_title: 'HOWTO: Force https/SSL for Apache2, Phusion Passenger and Rails'
author: matthew
post_date: 2010-10-26 00:39:38
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/howto-force-httpsssl-for-apache2-phusion-passenger-and-rails-1005.html
published: true
dsq_thread_id:
  - "162074609"
---
There's a lot of <a href="http://www.kalzumeus.com/2010/10/25/how-to-use-ssl-to-secure-your-rails-app-against-firesheep-and-other-evils/">buzz</a> right now about <a href="http://codebutler.com/firesheep">Firesheep</a> and non-secure Rails applications.

This is a pretty simple problem to solve with Apache's <code>mod_rewrite</code>. If the traffic isn't on https, force it to be. This configuration only needs to be in production, of course. 

Here's <code>/etc/apache2/sites-enabled/adgrok</code>:

<!--more-->

<pre lang="apache">
<VirtualHost *:80>
    ServerName secure.adgrok.com
    RewriteEngine On
    RewriteCond %{HTTPS} off
    RewriteRule (.*) https://secure.adgrok.com%{REQUEST_URI}
    NameVirtualHost *:443
</VirtualHost>
<IfModule mod_ssl.c>
<VirtualHost *:443>
        SSLEngine on
        ServerAdmin webmaster@adgrok.com
        ServerName secure.adgrok.com
        SSLCertificateFile /etc/ssl/private/secure.adgrok.com.crt
        SSLCertificateKeyFile /etc/ssl/private/adgrok.key
        SSLCertificateChainFile /etc/ssl/private/sf_bundle.crt
	SSLProtocol all
	SSLCipherSuite HIGH:MEDIUM
        DocumentRoot /u/apps/adgrok/current/public
        <Directory /u/apps/adgrok/current/public>
                Options -MultiViews
                AllowOverride all
        </Directory>
        ErrorLog /var/log/apache2/error.log
        LogLevel info
        CustomLog /var/log/apache2/access.log combined
</VirtualHost>
</pre>

Note that all of our CSS references relative paths, and we use named routes in our rails views, which takes care of the other URLs.