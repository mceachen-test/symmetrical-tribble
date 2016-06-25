---
ID: 22
post_title: Maildir auto-archive
author: matthew
post_date: 2008-04-12 21:43:10
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/maildir-auto-archive-22.html
published: true
btc_comment_counts:
  - 'a:0:{}'
btc_comment_summary:
  - 'a:0:{}'
dsq_thread_id:
  - "162691462"
---
<a href="http://www.gnu.org/licenses/gpl.html"><img style="float:right;padding:2em" src="http://matthew.mceachen.us/gplv3-127x51.png" alt="GPLv3" /></a>
If you've got your mail sitting on some server and in <a href="http://en.wikipedia.org/wiki/Maildir">Maildir</a> format, and you've used Outlook's "Auto Archive" feature, you might wish that your inbox (and subdirectory contents) could be automatically swept clean of items older than, say, 3 weeks, and shoved into a Year/Quarter sub folder (like "Inbox/2008/Q1/").

A couple years ago I wanted this too. So I wrote a cronjob and perl script to make this happen.

Installation is straightforward:
<ol>
	<li>Copy the shell script that cron calls, <a href="http://matthew.mceachen.us/geek/auto-archive/autoarchive">autoarchive</a>, and the perl script, <a href="http://matthew.mceachen.us/geek/auto-archive/autoarchive.pl">autoarchive.pl</a>,  onto the server holding your Maildir.</li>
	<li>Edit the autoarchive shellscript to make sure the path and --max-days is ok with you.</li>
	<li>Make both scripts executable with <code>chmod u+x</code></li>
	<li><strong>Make a backup of your mail</strong>. This is <a href="http://www.gnu.org/licenses/gpl.html">GPL</a> code. No warranty is implied. Read the code and try it out with --dry-run <strong>first</strong>.</li>
	<li>Calling autoarchive.pl with the "--dry-run" option will let you make sure it's doing what you want it to do. If it does, delete the dry-run.</li>
	<li>Wire it up to cron with something like:
<pre>PATH=$HOME/bin:/usr/bin:/bin
42 10 * * * autoarchive &gt; /dev/null</pre>
</li>
</ol>