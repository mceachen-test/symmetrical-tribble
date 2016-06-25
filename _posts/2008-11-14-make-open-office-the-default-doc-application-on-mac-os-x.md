---
ID: 77
post_title: 'Make OpenOffice the default &#8220;.doc&#8221; and &#8220;.xls&#8221; application on Mac OS X'
author: matthew
post_date: 2008-11-14 17:48:02
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/make-open-office-the-default-doc-application-on-mac-os-x-77.html
published: true
btc_comment_counts:
  - 'a:0:{}'
btc_comment_summary:
  - 'a:0:{}'
dsq_thread_id:
  - "160993344"
---
<img style="float:right;padding:.5em" src="http://matthew.mceachen.us/blog/wp-content/uploads/2008/11/default-mac-app.png"/><a href="http://download.openoffice.org/index.html">OpenOffice 3.0</a> is now out for the Mac. Finder opens files that end in ".doc" with TextEdit by default, and "Open with..." doesn't seem to stick. What to do?
<ol>
	<li>In the finder, find a file that ends in ".doc",</li>
	<li>Click once to select it</li>
	<li>Choose <tt>File &gt; Get Info...</tt></li>
	<li>Find the "Open with:" section, and click the triangle to show the contents of that section.</li>
	<li>Choose OpenOffice</li>
	<li>Click "Change All..."</li>
</ol>
You'll probably want to do this with <code>.ppt</code>, <code>.xls</code>, and <code>.csv</code>, too.

It was an odd choice to put the system-default web browser in a Safari preference pane and make other file types second-class citizens. Why not be consistent and make a System Preference pane for default applications?