---
ID: 270
post_title: >
  Quick MacPorts Installation of Ruby on
  Rails
author: matthew
post_date: 2009-01-24 02:17:15
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/quick-macports-installation-of-ruby-on-rails-270.html
published: true
syntaxhighlighter_encoded:
  - "1"
dsq_thread_id:
  - "160998936"
---
<div class="message"><strong>October 2010 update:</strong> I wrote this a while back, before I knew about RVM. The Ruby Version Manager has a bunch of features that makes it better than macports for ruby and gem installation. <a href="upgrading-to-rails-3-on-mac-os-x-and-ubuntu-966.html"><strong>Read about it here</strong></a>.</div>

I finally got to walking through a great <a href="http://www.alistapart.com/articles/gettingstartedwithrubyonrails">gentle introductory ALA article on Ruby on Rails</a>. I wanted to run <code>rails Demo</code> to build my first project, but I got nastiness:
<pre>$ <strong>rails -v</strong>
/System/Library/Frameworks/Ruby.framework/Versions/1.8/usr/lib/ruby/1.8/rubygems.rb:379:in <code>report_activate_error': RubyGem version error: rake(0.7.3 not &gt;= 0.8.3) (Gem::LoadError)
        from /System/Library/Frameworks/Ruby.framework/Versions/1.8/usr/lib/ruby/1.8/rubygems.rb:311:in </code>activate'
        from /System/Library/Frameworks/Ruby.framework/Versions/1.8/usr/lib/ruby/1.8/rubygems.rb:337:in <code>activate'
        from /System/Library/Frameworks/Ruby.framework/Versions/1.8/usr/lib/ruby/1.8/rubygems.rb:336:in </code>each'
        from /System/Library/Frameworks/Ruby.framework/Versions/1.8/usr/lib/ruby/1.8/rubygems.rb:336:in <code>activate'
        from /System/Library/Frameworks/Ruby.framework/Versions/1.8/usr/lib/ruby/1.8/rubygems.rb:65:in </code>active_gem_with_options'
        from /System/Library/Frameworks/Ruby.framework/Versions/1.8/usr/lib/ruby/1.8/rubygems.rb:50:in `gem'
        from /usr/bin/rails:18</pre>
That's not especially welcoming. Let's see if MacPorts can come to the rescue.

After <a href="http://www.macports.org/install.php">installing MacPorts</a>, this got me going:
<pre><strong>sudo port selfupdate
sudo port install rb-rubygems
sudo gem install rails
</strong></pre>
I saw this from the gem install rails:
<pre>Successfully installed rake-0.8.3
Successfully installed activesupport-2.2.2
Successfully installed activerecord-2.2.2
Successfully installed actionpack-2.2.2
Successfully installed actionmailer-2.2.2
Successfully installed activeresource-2.2.2
Successfully installed rails-2.2.2
7 gems installed
Installing ri documentation for rake-0.8.3...
Installing ri documentation for activesupport-2.2.2...
Installing ri documentation for activerecord-2.2.2...
Installing ri documentation for actionpack-2.2.2...
Installing ri documentation for actionmailer-2.2.2...
Installing ri documentation for activeresource-2.2.2...
Installing RDoc documentation for rake-0.8.3...
Installing RDoc documentation for activesupport-2.2.2...
Installing RDoc documentation for activerecord-2.2.2...
Installing RDoc documentation for actionpack-2.2.2...
Installing RDoc documentation for actionmailer-2.2.2...
Installing RDoc documentation for activeresource-2.2.2...</pre>
I had some issues because my PATH included /usr/bin before /opt/local/bin (so MacOS's old gem binary got picked up before MacPorts' version). You can tell you're running the correct version:
<pre><strong>$ which gem</strong>
/opt/local/bin/gem
$ <strong>gem -v
</strong>1.3.1</pre>
If you see version 1.0.1, you're still need to <a href="http://guide.macports.org/#installing.shell.postflight">follow these instructions to install MacPorts</a>. Add something like this to your <tt>~/.bashrc</tt> or <tt>~/.profile</tt>:
<pre class="lang:bash decode:1 " >export PATH=/opt/local/bin:/opt/local/sbin:$PATH</pre>

Anyway, looks like things are good now. Yea new toys.