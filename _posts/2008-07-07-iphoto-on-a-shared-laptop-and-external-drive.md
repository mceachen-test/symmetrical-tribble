---
ID: 56
post_title: >
  50,000 photos in iPhoto on a shared
  laptop and external drive
author: matthew
post_date: 2008-07-07 06:10:01
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/iphoto-on-a-shared-laptop-and-external-drive-56.html
published: true
syntaxhighlighter_encoded:
  - "1"
btc_comment_counts:
  - 'a:0:{}'
btc_comment_summary:
  - 'a:0:{}'
dsq_thread_id:
  - "160993308"
---
I've got over 100GiB of photos (JPEG and RAW) taken over the years, and they don't comfortably fit on a laptop. The laptop is also shared by everyone in my family, each with their own account, so what to do?

I found out that if you hold down the option key when you start iPhoto it asks for the location of your iPhoto Library -- so that takes care of the first problem -- just plug in a big external hard drive and you're set.

There's one glitch to having the photos on an external drive, though -- Time Machine ignores exernal drives by default. Go into System Preferences... &gt; Time Machine. Click "Options". You'll see your external drive in the "Do not back up:" list. Click the external drive holding your photos, then click the minus button.

The next problem is how to share the library with others. That's described quite well by a <a href="http://www.macosxhints.com/article.php?story=20050904072808460&amp;lsrc=osxh">Mac OS X Hint</a>. <strong>Note that the following assumes you've named your external hard drive "Photos".</strong>

Step 1: Enable ACLs:
<pre class="lang:bash decode:1 " >
sudo fsaclctl -p /Volumes/Photos -e
</pre>

Step 2: Add the people that can use iPhoto. <strong>Replace "USERNAME" with the short login name of the people that you want to be able to use iPhoto</strong>:
<pre class="lang:bash decode:1 " >
sudo chmod +a &quot;USERNAME allow read,write,delete,list,search,add_file,add_subdirectory,delete_child,file_inherit,directory_inherit&quot; \
  /Volumes/Photos/iPhoto\ Library
</pre>
Creating a group account, subscribing the users to that group, and letting group have read/write/execute to the directory should work, but the umask may not be 775. It's something to check if this doesn't keep working.