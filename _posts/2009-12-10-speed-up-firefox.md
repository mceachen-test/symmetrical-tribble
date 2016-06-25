---
ID: 751
post_title: Speed up Firefox!
author: matthew
post_date: 2009-12-10 20:42:04
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/speed-up-firefox-751.html
published: true
syntaxhighlighter_encoded:
  - "1"
dsq_thread_id:
  - "160993539"
---
My Firefox on Mac was getting pretty lethargic -- almost a minute to spin up, and multiple seconds to just open a new tab. I installed the new beta of Google Chrome, and remembered how nice a speedy browser was.

<a href="http://www.google.com/chrome">Google Chrome for Mac</a> is a nice first effort, but without custom search engines, and all the other firefox add-ons, the shininess gets pretty tarnished. I needed my fast firefox back!

I made just a couple changes, though, and my Firefox is back to it's prior speedy self!

<!--more-->
<h3>Disable site verification</h3>
The first step is easy (but it <strong>should only be done with caution</strong>): Go to Firefox &gt; Preferences &gt; Security, and disable Block reported attack sites and Block reported web forgeries.

<strong>Do this at your own risk</strong>. If you're already using <a href="http://www.opendns.com/">OpenDNS</a>, you're already have some security from badware (and it's works everywhere, not just with firefox).
<h3>Put your databases on a diet</h3>
The other slowdown I found was due to firefox's internal databases getting fragmented. 

First, if you don't want or need your prior browser history, select Tools... &gt; Clear Recent History... and delete what you can.

Then vacuum the databases. There are two ways to do this -- you can use the <a href="https://addons.mozilla.org/en-US/firefox/addon/13878">Vacuum Places</a> firefox add-on (which only vacuums the places db), or you can do it by hand, and vacuum all the databases.

To vacuum manually, first install <a href="http://www.macports.org">macports</a>, shut down firefox, and run this script:

<pre class="lang:bash decode:1 " >
sudo port install sqlite3
cd ~/Library/Application\ Support/Firefox/Profiles/*
for i in *sqlite ; do sqlite3 $i &quot;VACUUM;&quot; ; done
</pre>

Vacuuming defragments and re-indexes the internal firefox databases. If you deleted history entries, this will shrink those databases dramatically, reducing the time firefox takes to load those databases in the future.