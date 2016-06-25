---
ID: 123
post_title: 'Make &#8220;ps -ef&#8221; work in a shell on Mac OS X'
author: matthew
post_date: 2008-09-10 21:47:05
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/make-ps-ef-work-on-mac-os-x-123.html
published: true
btc_comment_summary:
  - 'a:0:{}'
btc_comment_counts:
  - 'a:0:{}'
dsq_thread_id:
  - "161192288"
---
If you're used to SunOS or BSD, you'll be at home with Mac OS X's "<code>ps -aux</code>" to get a process list from a shell prompt.

If you've been using any other recent unix, though, your fingers will want to type <code>ps -ef</code> instead. Rather than hack an alias to wrap <code>ps</code> to make this happen, it turns out there's an easy way to return to the ps promised lands.

By default on Mac OS X 10.5.2, the shell environment's <code>COMMAND_MODE</code> is set to <code>legacy</code>. If you set it to <code>unix2003</code>, you'll get your <code>ps -ef</code>. Just add 

<pre>export COMMAND_MODE=unix2003
alias zcat='gunzip -c'
</pre>

to your <code>~/.bashrc</code> to make it be set automatically.

The <code>alias</code> of <code>zcat</code> to <code>gunzip -c</code> fixes a "feature" in unix2003 mode -- it <em>removes gzip support from zcat</em>. If you're used to using <code>zcat</code> for both <code>compress</code>ed <code>.Z</code> files as well as <code>gzip</code>ped <code>.gz</code> files, you want the alias line as a workaround.