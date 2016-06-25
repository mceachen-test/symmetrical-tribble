---
ID: 25
post_title: Bad Blocks Make Macs Unhappy
author: matthew
post_date: 2008-04-16 07:41:40
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/checking-for-bad-blocks-on-mac-os-25.html
published: true
btc_comment_counts:
  - 'a:0:{}'
btc_comment_summary:
  - 'a:0:{}'
dsq_thread_id:
  - "162291901"
---
My fairly young MacBook Pro started randomly hanging, not coming out of sleep, and being generally disagreeable a couple days ago, and it turned out to be a bad hard disk. 

What's disconcerting is that Disk Utility.app <strong>didn't see any problem with the disk</strong>. I had to install <a href="http://smartmontools.sourceforge.net/">smartmontools</a> to find the error.

After installing <a href="http://www.macports.org">MacPorts</a>, install smartmontools:
<pre>sudo port install smartmontools</pre>

Then tell the drive to do a long self-check in the background:
<pre>sudo smartctl -t long /dev/disk0</pre>

Note that this check may take an hour to run, but it's done in the background, so you can continue to use your computer while it does its little dance on the catwalk.

Check the status of the test with:
<pre>sudo smartctl -c /dev/disk0</pre>

I was <i>unlucky</i>:

<pre>
...
Self-test execution status: ( 121) The previous self-test completed having
  the read element of the test failed.
...
</pre>

See <a href="http://forums.macosxhints.com/showthread.php?t=79643">The macosxhints forums</a> for more discussion about this issue.