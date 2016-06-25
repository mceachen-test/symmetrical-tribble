---
ID: 966
post_title: >
  Switching between Rails 2 and Rails 3 on
  Mac OS X or Ubuntu with RVM
author: matthew
post_date: 2010-10-23 21:20:24
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/switching-between-rails-2-and-rails-3-on-mac-os-x-or-ubuntu-with-rvm-966.html
published: true
btc_comment_summary:
  - 'a:0:{}'
dsq_thread_id:
  - "161013432"
---
I'm migrating <a href="http://adgrok.com">AdGrok</a> to Rails 3 this weekend--what with the new ActiveRecord query functionality, and MongoMapper, and all our gems migrating to Rails 3, it seems like it's time.

The <a href="http://railscasts.com/episodes/200-rails-3-beta-and-rvm">Railscast</a> (and <a href="http://asciicasts.com/episodes/200-rails-3-beta-and-rvm">ASCIIcast</a>) is great, but a couple steps are now outdated. There also aren't any instructions on how to switch between rails 2 and rails 3 gracefully.

<!--more-->

<h2>Install RVM</h2>

The <a href="http://rvm.beginrescueend.com/">Ruby Version Manager</a> (rvm) makes it easy to switch between versions of ruby as well as sets of gems. When you upgrade your rails app from 2.x to 3.x, this can be <b>really</b> handy.

One thing to note: RVM places all files in <tt>~/.rvm</tt>. <strong>Run all these commands as you</strong>. Don't use sudo with rvm or gem, (unless you're doing a system-wide RVM installation, of course).

If you're on Ubuntu, you'll need curl, git, build-essentials, and a bunch of develpoment libraries before you can continue:
<pre lang="bash">sudo apt-get install curl git-core build-essential libreadline-dev zlib1g-dev libssl-dev libxslt1-dev </pre>

If you're using MySQL, add <code>libmysqlclient-dev </code> to that list.

 To install RVM, run this in your terminal (<a href="http://rvm.beginrescueend.com/rvm/install/">as per the instuctions</a>):
<pre lang="bash">
bash < <( curl http://rvm.beginrescueend.com/releases/rvm-install-head )
</pre>
Then add this to the end of your <tt>~/.bashrc</tt>:
<pre lang="bash">
[[ -s "$HOME/.rvm/scripts/rvm" ]] && source "$HOME/.rvm/scripts/rvm"
</pre>
And open a new shell. Test that <code>rvm</code> is installed by running <b>rvm -v</b>.

<h3>Install ruby 1.9.2</h3>

<pre lang="bash">rvm install 1.9.2</pre>
(pretty slick, eh?) and make it the default ruby:
<pre lang="bash">rvm 1.9.2 --default</pre>

Test that you're on the right ruby: <b>ruby -v</b>

<h3>Install Rails 3</h3>

<pre lang="bash">rvm gem install rails</pre>

Boom! You're done! Test with <b>rails -v</b></t>.

<h3>Install Rails 2</h3>

If you want to run rails 2.x for one app in one shell and rails 3 in another, it's pretty easy:

<pre lang="bash">
rvm install 1.8.7
rvm use 1.8.7
rvm gem install -v=2.3.8 rails
</pre>

and presto, you've got a 2.3.8 environment:

<pre lang="bash">
$ ruby -v
ruby 1.8.7 (2010-08-16 patchlevel 302) [i686-darwin10.4.0]
$ rails -v
Rails 2.3.8
</pre>
And switch back to rails 3:
<pre lang="bash">
$ rvm use 1.9.2
Using /Users/mrm/.rvm/gems/ruby-1.9.2-p0
$ rails -v
Rails 3.0.1
$ ruby -v
ruby 1.9.2p0 (2010-08-18 revision 29036) [x86_64-darwin10.4.0]
</pre>

<h3>If you're upgrading from macports...</h3>

If you're starting from scratch, the preceding steps should work. I was upgrading from rails 2.3.x and ruby 1.8.x installed via MacPorts, however, and saw them not play nicely together.

In switching to rvm, I ended up completely nuking my MacPorts installation (<a href="http://guide.macports.org/#installing.macports.uninstalling">as per the instructions</a>) -- I ended up only really needing wget and dsh, and those reinstalled simply), because I had an <code>/opt/local/bin/rails</code> that wasn't owned by any installed port (who knows how it got there), and tons of old gems that I didn't want to pull in accidentally.

For the benefit of the googles, here's the errors I saw when the rvm ruby superceded the macports ruby:
<pre>
$ rails
-bash: /opt/local/bin/rails: /opt/local/bin/ruby: bad interpreter: No such file or directory
</pre>

And <code>/usr/bin/rails</code> (again, I don't know where that one came from--Mac OS X?), barfed thusly:
<pre>
$ rails
/Library/Ruby/Site/1.8/rubygems.rb:779:in `report_activate_error': Could not find RubyGem rails (>= 0) (Gem::LoadError)
	from /Library/Ruby/Site/1.8/rubygems.rb:214:in `activate'
	from /Library/Ruby/Site/1.8/rubygems.rb:1082:in `gem'
	from /usr/bin/rails:18
</pre>

After I installed rvm properly (so the ~/.rvm paths are before system paths), and ran <tt>rvm gem install rails</tt>, everything worked.

<h3>If you're installing on Ubuntu...</h3>

and you see this:

<pre>
Error running './configure --prefix=/usr/local/rvm/rubies/ruby-1.8.7-p302 --enable-shared  ', please check /usr/local/rvm/log/ruby-1.8.7-p302/configure.error.log
There has been an error while running configure. Halting the installation.
</pre>

and configure.error.log complains about "configure: error: no acceptable C compiler found in $PATH" -- you need to install the "build-essential" package:

<pre lang="bash">sudo apt-get install build-essential</pre>

If you run "rails console", and see ... <tt>lib/ruby/1.8/irb/completion.rb:10:in `require': no such file to load -- readline (LoadError)</tt> -- you need to 

<pre lang="bash">
sudo apt-get install libreadline-dev
rvm remove 1.8.7
rvm install 1.8.7 
</pre>