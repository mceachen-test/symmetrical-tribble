---
ID: 1021
post_title: 'HOWTO: Make system-wide RVM installations work with cron, monit, delayed_job, and passenger'
author: matthew
post_date: 2010-11-04 21:02:17
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/howto-make-system-wide-rvm-installations-work-with-cron-monit-delayed_job-and-passenger-1021.html
published: true
dsq_thread_id:
  - "167411252"
---
<a href="http://rvm.beginrescueend.com/deployment/system-wide/">System-wide RVM installations</a> are recommended for production deployments, but because /usr/bin/ruby is no longer the "correct" ruby, you need to tell all your moving parts about RVM.

<!--more-->

<h3>Before installing ruby</h3>

Make sure you've installed the development libraries that ruby and all of your gems need to work. For <a href="http://adgrok.com">AdGrok</a>, that amounts to:

<pre lang="bash">sudo apt-get install curl git-core build-essential libreadline-dev zlib1g-dev libssl-dev libxslt1-dev libmysqlclient-dev</pre>

<h3>Finish the installation</h3>

After you install RVM, look at your <code>/etc/profile</code>.  If you already have this section:
<pre lang="bash">
if [ -d /etc/profile.d ]; then
  for i in /etc/profile.d/*.sh; do
    if [ -r $i ]; then
      . $i
    fi
  done
  unset i
fi
</pre> 

you just need to copy the following into a new file, <code>/etc/profile.d/rvm.sh</code>: 

<pre lang="bash">[[ -s "/usr/local/lib/rvm" ]] && source "/usr/local/lib/rvm"</pre>

I originally had this in <code>/etc/bash.bashrc</code>, but that broke the "bash -l" solution below (because "PS1" wasn't defined).
 
<h3>Fix rails runner crontabs</h3>

Cron's default shell, /bin/sh, doesn't load the "interactive shell" configuration, so RVM won't be available.

RVM comes with a "rvm-shell" wrapper script to fix this issue. Edit your crontab and change the SHELL. Here's my whole <code>/etc/cron.d/example.cron</code>:

<pre lang="bash">
PATH=/sbin:/bin:/usr/sbin:/usr/bin
SHELL=/usr/local/bin/rvm-shell
MAILTO=...
RAILS_ENV=production
# m h dom mon dow user command
# World's most inefficient way to tell the time:
*/15 * * * * deploy cd /u/apps/adgrok/current && rails runner "puts Time.now"
</pre>

<h3>Fix monit/delayed_job</h3>

<a href="http://groups.google.com/group/rubyversionmanager/browse_thread/thread/d1a6c1f6396a8bf6/51afece4c8943912?#51afece4c8943912">The author of RVM</a> suggested you could omit the "as uid ... and gid ..." and use a /bin/su prefix to load RVM, but I couldn't get it to work for me. I had to use the bash -l -c method, just like in my crontab:

<pre lang="bash">
check process delayed_job.0
  with pidfile /u/apps/adgrok/shared/pids/delayed_job.0.pid
  start program = "/usr/local/bin/rvm-shell -c 'RAILS_ENV=production /u/apps/adgrok/current/script/delayed_job start -i 0'" as uid deploy and gid deploy
  stop program = "/usr/local/bin/rvm-shell -c 'RAILS_ENV=production /u/apps/adgrok/current/script/delayed_job stop -i 0'" as uid deploy and gid deploy
  if 2 restarts within 15 cycles then timeout
</pre>

<h3>Fix Phusion Passenger</h3>

When you installed passenger, you specified a "<a href="http://www.modrails.com/documentation/Users%20guide%20Apache.html#PassengerRuby">PassengerRuby</a>" value. Change that from /usr/bin/ruby1.8 (or whatever), to RVM's ruby. In my case, it was 

<pre lang="bash">
PassengerRuby /usr/local/rvm/rubies/ruby-1.8.7-p302/bin/ruby
</pre>