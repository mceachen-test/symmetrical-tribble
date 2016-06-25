---
ID: 30
post_title: 'Running a command for all files whose name matches&#8230;'
author: matthew
post_date: 2008-04-20 22:14:06
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/running-a-command-for-all-files-whose-name-matches-30.html
published: true
btc_comment_counts:
  - 'a:0:{}'
btc_comment_summary:
  - 'a:0:{}'
dsq_thread_id:
  - "185976782"
---
I found a stray image named "img_1234.jpg" on a laptop and wanted to see if I already had it on my server.

On my Mac I could use spotlight's nifty "kind:image" filter along with <a href="http://www.apple.com/macosx/features/quicklook.html">quicklook</a>. <a href="http://www.macworld.com/article/132788/2008/04/spotlight2.html">Macworld has a great article about advanced spotlight usage</a>. 

On Ubuntu, it's almost as easy:

<pre>locate -i img_1234.jpg | xargs -d'\n' feh -F -d</pre>

<ul>
	<li>The <code>locate -i</code> says "find img_1234.jpg without case sensitivity. Locate likes to separate filenames (that might have spaces) with a newline.</li>
	<li>The <code>xargs -d'\n'</code> says "expect filenames that are are separated by newline</li>
	<li>The <code>feh -F -d</code> tells <a href="http://linuxbrit.co.uk/feh/">feh, a great little image viewer</a>, to reduce the image to fit to the screen and draw the filename.</li>
</ul>