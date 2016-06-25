---
ID: 107
post_title: >
  Automatic panoramas with hugin and
  autopano
author: matthew
post_date: 2008-08-08 18:51:58
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/automatic-panoramas-with-hugin-and-autopano-107.html
published: true
btc_comment_counts:
  - 'a:0:{}'
btc_comment_summary:
  - 'a:0:{}'
dsq_thread_id:
  - "160993364"
---
I've only been using <a href="http://hugin.sourceforge.net/">hugin</a> for an hour now, and I'm really impressed.

The mac binary doesn't include the ability to automatically determine how images overlap, but it's pretty easy to make that work, too.

First download the hugin binary. <a href="http://panorama.dyndns.org/index.php?lang=EN&amp;subject=Hugin&amp;texttag=Hugin">The Mac OS X port is here</a>.

Then install the autopano binary. 

First <a href="http://www.macports.org/install.php">install MacPorts</a>.

Then run:
<pre>
sudo port selfupdate
sudo port install autopano-sift-c
</pre>

I had a problem with cmake hanging during install. If port stalls for you, ctrl-c, then <code>sudo port clean cmake ; sudo port install cmake ; sudo port install autopano-sift-c</code> again.

Then tell Hugin where the binary is. Go to Hugin &gt; Preferences... and click the "Autopano" tab, and make it look like this:

<img class="alignnone size-full wp-image-108" title="hugin-autopano" src="http://matthew.mceachen.us/blog/wp-content/uploads/2008/08/hugin-autopano.jpg" alt="" width="493" height="470" />

Close the preferences window, and the next time you use the "Assistant" tab and load images, you'll see that the alignment is done for you automatically.

Here's an example taken from Bryce Point in <a href="http://www.nps.gov/brca/">Bryce Canyon</a>:<a href="http://matthew.mceachen.us/blog/wp-content/uploads/2008/08/bryce-amphitheatre-pan-1.jpg"><img class="alignnone size-thumbnail wp-image-111" title="bryce-amphitheatre-pan-1" src="http://matthew.mceachen.us/blog/wp-content/uploads/2008/08/bryce-amphitheatre-pan-1.jpg" alt="" width="500" /></a> (it's also one of the banners).