---
ID: 1159
post_title: >
  How to wrangle the pg (postgresql) gem
  with macports and rvm
author: matthew
post_date: 2011-05-21 16:13:35
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/howto-wrangle-the-pg-postgresql-gem-with-macports-and-rvm-1159.html
published: true
dsq_thread_id:
  - "310135955"
---
Seeing this?

<pre>
Installing pg (0.11.0) with native extensions /Users/mrm/.rvm/rubies/ruby-1.9.2-p180/lib/ruby/site_ruby/1.9.1/rubygems/installer.rb:533:in `rescue in block in build_extensions': ERROR: Failed to build gem native extension. (Gem::Installer::ExtensionBuildError)

        /Users/mrm/.rvm/rubies/ruby-1.9.2-p180/bin/ruby extconf.rb 
checking for pg_config... no
No pg_config... trying anyway. If building fails, please try again with
 --with-pg-config=/path/to/pg_config
checking for libpq-fe.h... no
Can't find the 'libpq-fe.h header
*** extconf.rb failed ***
</pre>

Wondering how or where to get this mythical <code>pg_config</code>? It's part of the PostgreSQL package. If you're running <a href="http://www.macports.org/">MacPorts</a>, it's easy:

<pre lang="bash">sudo port install postgresql90</pre>

Then, following the instructions on the <a href="http://beginrescueend.com/integration/databases/">RVM website</a>, run

<pre lang="bash">gem install pg -- --with-pg-config=/opt/local/lib/postgresql90/bin/pg_config</pre>