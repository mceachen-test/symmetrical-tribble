---
ID: 65
post_title: '&#8220;Backing up iPhone&#8221; taking forever? Reset iSync!'
author: matthew
post_date: 2008-07-14 23:43:38
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/backing-up-iphone-taking-forever-reset-isync-65.html
published: true
btc_comment_summary:
  - 'a:0:{}'
btc_comment_counts:
  - 'a:0:{}'
dsq_thread_id:
  - "160993323"
---
<strong>Update 9/14/2008:

Backup speed was addressed in iPhone software version 2.1.</strong> Launch iTunes, dock your iPhone, and click "Check for Update".

<em>And now back to the original article:</em>
<hr>

I was surprised to see my brand-new iPhone 3G taking more than 15 minutes to synchronize with iTunes. It would get "stuck" in "Backing up iPhone..."

It looks like there are two reasons why the backup process takes longer -- iTunes backing up newly-installed applications, and <a href="http://discussions.apple.com/thread.jspa?messageID=7546002#7579292">MobileMe/iSync database cruftiness</a>. Expect sync'ing to take a while if you've installed new applications. If you haven't installed new applications, you should try resetting the iSync database.

It's pretty simple:

<img class="size-full wp-image-66 alignright" style="float:right" title="reset-sync-history" src="http://matthew.mceachen.us/blog/wp-content/uploads/2008/07/reset-sync-history.png" alt="" width="257" height="212" />
<ol>
	<li>Close iTunes.</li>
	<li>Start iSync</li>
	<li>Click iSync &gt; Preferences.</li>
	<li>Click "Reset Sync History...</li>
</ol>
The next time you sync with iTunes, the "Backing up" step should be dramatically shorter.
<div style="clear:both">

Another interesting solution involves <a href="http://tsunanet.blogspot.com/2008/04/iphoneical-sync-woes-datasyncdb-getting.html">Vacuuming your syncdb</a>. If you've got <a href="http://www.macports.org/">MacPorts</a> installed, it's easy:
<pre>sudo port install sqlite3
sqlite3 \
  ~/Library/Application\ Support/SyncServices/Local/data.syncdb\
  vacuum</pre>
</div>