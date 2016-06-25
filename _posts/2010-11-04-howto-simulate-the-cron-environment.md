---
ID: 1018
post_title: 'HOWTO: simulate the cron environment'
author: matthew
post_date: 2010-11-04 19:28:29
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/howto-simulate-the-cron-environment-1018.html
published: true
dsq_thread_id:
  - "167382295"
---
While banging my head on RVM + Rails 3 + crontabs, it became clear that I needed the cron environment in an interactive shell. It's not hard:

<pre lang="bash">sudo su
env -i /bin/sh</pre>

If your SHELL in your crontab is /bin/bash:

<pre lang="bash">sudo su
env -i /bin/bash --noprofile --norc</pre>

Remember that the cron-invoked shell won't have a tty, so some commands will behave differently. Also, if you've got a bunch of other environment variables set up in your crontab, you can <a href="http://stackoverflow.com/questions/2135478/how-to-simulate-the-environment-cron-executes-a-script-with/2546509#2546509">do this trick from stackoverflow</a> -- create a one-off cron that writes env to a file, and load that env later.