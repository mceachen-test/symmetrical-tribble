---
ID: 403
post_title: >
  How to clean up photos of whiteboards
  with Gimp
author: matthew
post_date: 2009-05-29 02:08:45
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/how-to-clean-up-photos-of-whiteboards-with-gimp-403.html
published: true
dsq_thread_id:
  - "161686158"
---
<img class="alignright size-full wp-image-405" title="before" src="http://matthew.mceachen.us/blog/wp-content/uploads/2009/05/before.png" alt="before" width="250" height="182" />Here is a thumbnail of a picture of a whiteboarding session taken with my iPhone. It's basically unreadable. I looked for a good recipe to clean this up in gimp, but the googles failed me. 

<div style="clear:both"></div>
After playing with a bunch of stuff with the gimp, here are the steps that I've found to work pretty well:
<ol>
	<li>Load the image in <a href="http://www.gimp.org/">gimp</a></li>
	<li style="clear:both">Now for the magicks: Choose Filters &gt;Edge Detect &gt; Difference of Gaussians...<img class="alignright size-full wp-image-409" title="diff-of-gauss" src="http://matthew.mceachen.us/blog/wp-content/uploads/2009/05/diff-of-gauss.png" alt="diff-of-gauss" width="281" height="150" /></li>
	<li style="clear:both">Start with these settings. It doesn't make it perfect, but it should make the lines stand out much better. Play around with the radius 1 and radius 2 values if you aren't happy. Radius 1 needs to be bigger than Radius 2. Click OK.</li>
	<li style="clear:both"><img src="http://matthew.mceachen.us/blog/wp-content/uploads/2009/05/final-levels.png" alt="final-levels" title="final-levels" width="301" height="368" class="alignright size-full wp-image-411" /> Return to  Colors &gt; Levels... and move both the blackpoint and the midpoint to the right (to make the lines stand out) and the whitepoint to the left just a bit (to make the whiteboard white). Click OK.</li>
</ol>

<div style="clear:both"></div>

Here's the result:

<img class="alignright size-full wp-image-404" title="after" src="http://matthew.mceachen.us/blog/wp-content/uploads/2009/05/after.png" alt="after" width="250" height="182" />