---
ID: 861
post_title: 'Installing Phusion Passenger on Ruby 1.9.1, Nginx, &#038; Ubuntu 10.04'
author: matthew
post_date: 2010-05-17 16:27:36
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/installing-phusion-passenger-on-ruby-1-9-1-nginx-ubuntu-10-04-861.html
published: true
syntaxhighlighter_encoded:
  - "1"
dsq_thread_id:
  - "161010424"
---
<div class="message">I wrote this a while back, before I knew about RVM. The Ruby Version Manager has a bunch of features that makes it better than macports for ruby and gem installation. <a href="upgrading-to-rails-3-on-mac-os-x-and-ubuntu-966.html"><strong>Read about it here</strong></a>.</div>


Getting ruby 1.9.1 and <a href="http://nginx.org/en/">nginx</a> and <a href="http://www.modrails.com/install.html">passenger</a> and ubuntu to all play nicely is fairly straightforward, but it's not just "apt-get" and "gem install" lovin'.

Making ruby 1.9.1 the default ruby is OK. Follow <a href="http://michalf.me/blog:make-ruby-1-9-default-on-ubuntu-9-10-karmic-koala">these steps</a>:

<!--more-->

<h3>Install ruby 1.9.1</h3>

<pre class="lang:bash decode:1 " >
sudo apt-get update

sudo apt-get install ruby1.9.1 ruby1.9.1-dev \
  rubygems1.9.1 irb1.9.1 ri1.9.1 rdoc1.9.1 \
  build-essential nginx libopenssl-ruby1.9.1 libssl-dev zlib1g-dev

sudo update-alternatives --install /usr/bin/ruby ruby /usr/bin/ruby1.9.1 400 \
         --slave   /usr/share/man/man1/ruby.1.gz ruby.1.gz \
                        /usr/share/man/man1/ruby1.9.1.1.gz \
        --slave   /usr/bin/ri ri /usr/bin/ri1.9.1 \
        --slave   /usr/bin/irb irb /usr/bin/irb1.9.1 \
        --slave   /usr/bin/rdoc rdoc /usr/bin/rdoc1.9.1

sudo update-alternatives --config ruby
</pre>

OK, now that we've got ruby 1.9.1 and g++, we can build passenger:

<pre class="lang:bash decode:1 " >
sudo gem install passenger
</pre>

But that throws the executable "passenger-install-nginx-module" into  a subdirectory. Exec from there:

<pre class="lang:bash decode:1 " >
cd /var/lib/gems/1.9.1/gems/passenger-2.2.11/bin
sudo ./passenger-install-nginx-module
</pre>

And according to the build script, we're done!

But we aren't. At least for me, I needed to

<h3>Fix up the configurations</h3>

First up <tt>/etc/init.d/nginx</tt> in a sudo'ed editor, and replace these two lines:

<pre class="lang:bash decode:1 " >
PATH=/opt/nginx/sbin:/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin
DAEMON=/opt/nginx/sbin/nginx
</pre>

If you fail to do this, you'll see this error when you run <tt>/etc/init.d/nginx restart</tt>: 

<pre>$ sudo /etc/init.d/nginx start
Starting nginx: [emerg]: unknown directive "passenger_enabled" in /etc/nginx/sites-enabled/default:32
configuration file /etc/nginx/nginx.conf test failed
</pre>

This is because /usr/sbin/nginx doesn't know how to dance Phusion, hence the need to update the init script.

(TODO: there should be an update-alternatives solution to this, but I can't find a recipe to replace an existing binary)

You also may see this:

<pre>Restarting nginx: the configuration file /opt/nginx/conf/nginx.conf syntax is ok
configuration file /opt/nginx/conf/nginx.conf test is successful
[emerg]: bind() to 0.0.0.0:80 failed (98: Address already in use)
[emerg]: bind() to 0.0.0.0:80 failed (98: Address already in use)
[emerg]: bind() to 0.0.0.0:80 failed (98: Address already in use)
[emerg]: bind() to 0.0.0.0:80 failed (98: Address already in use)
[emerg]: bind() to 0.0.0.0:80 failed (98: Address already in use)
[emerg]: still could not bind()
nginx.
</pre>

Something (most likely apache2) is already listening on port 80. Find out with <tt>lsof</tt>:

<pre class="lang:bash decode:1 " >$ sudo lsof -i tcp:80
COMMAND   PID   USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
nginx   14790   root    6u  IPv4  29964      0t0  TCP *:www (LISTEN)
nginx   14798 nobody    6u  IPv4  29964      0t0  TCP *:www (LISTEN)
</pre>

If it's apache, edit <tt>/etc/apache2/ports.conf</tt> and shove it to some other port (like 8888) and run <tt>/etc/init.d/apache2 restart</tt>:

<pre>
NameVirtualHost *:8888
Listen 8888
</pre>

<h3>Add your rails configuration</h3>

In <tt>/etc/nginx/sites-enabled/default</tt>, in the <tt>server { ... }</tt> section, add

<pre>
	location /underpants {
		root /webapps/underpants/public;
		passenger_enabled on;
	}
</pre>

and restart nginx.