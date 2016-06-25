---
ID: 332
post_title: >
  How to set up native subversion (javahl)
  with Subclipse on Mac OS X
author: matthew
post_date: 2009-02-23 13:33:56
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/how-to-set-up-native-subversion-javahl-with-subclipse-on-mac-os-x-332.html
published: true
syntaxhighlighter_encoded:
  - "1"
dsq_thread_id:
  - "160993412"
---
Note that this is for Ganymede (Eclipse 3.4.x).

<ol>
	<li>Install the javahl binding with MacPorts: 
<pre>sudo port install subversion +bash_completion
sudo port install subversion-javahlbindings
</pre></li>

	<li>Run eclipse, and add this upgrade site: http://subclipse.tigris.org/update_1.4.x</li>
	<li>Select the "JavaHL Adapter" and "Subclipse" modules and click "Install"</li>
	<li>Choose Eclipse > Preferences..., go to Team > SVN, find the "SVN Interface" pulldown, and choose "JavaHL"</li>
</ol>