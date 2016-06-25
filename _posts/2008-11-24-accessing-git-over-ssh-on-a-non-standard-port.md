---
ID: 217
post_title: >
  Accessing git over ssh on a non-standard
  port
author: matthew
post_date: 2008-11-24 14:14:46
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/accessing-git-over-ssh-on-a-non-standard-port-217.html
published: true
syntaxhighlighter_encoded:
  - "1"
btc_comment_summary:
  - 'a:0:{}'
btc_comment_counts:
  - 'a:0:{}'
dsq_thread_id:
  - "161630426"
---
Simple ssh access to a git repo can be sufficient for a small dev team--but what if you're using a non-standard ssh port?

<a href="http://infovore.org/archives/2008/10/13/pulling-from-git-over-a-non-standard-ssh-port/">The solution</a>--do as Linus says. Use <tt>~/.ssh/config</tt>.

My config now looks like this:
<pre>Host my.servername.org
  Port 1234
</pre>
Remember that <code>~/.ssh</code> needs to be 700 (read/write/execute for only the owner), and the files inside are all 600:
<pre>$ chmod 700 ~/.ssh
$ chmod 600 ~/.ssh/*
</pre>