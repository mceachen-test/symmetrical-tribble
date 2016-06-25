---
ID: 721
post_title: How to fix your ubuntu 9.10 console
author: matthew
post_date: 2009-11-28 22:19:56
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/how-to-fix-your-ubuntu-9-10-console-721.html
published: true
syntaxhighlighter_encoded:
  - "1"
dsq_thread_id:
  - "163924443"
---
After upgrading my server (that isn't running X) to <a href="http://ubuntu.com">Ubuntu</a> 9.10 (Karmic Koala), I found that the console on my LCD monitor was cropping out several characters from the left and right sides. It wasn't hard to fix, though, given the magick incantations:

<!--more-->

<h3>If you upgraded to Karmic</h3>
You'll have grub 1 (legacy). To fix it, follow <a href="http://ubuntuforums.org/showthread.php?t=186671">this thread</a>, and edit <code>/boot/grub/menu.lst</code>. Look for the line that starts with "defoptions" -- uncomment the line and add "vga=791" (or 789 for lower resolution):
<pre class="lang:bash decode:1 " >
## additional options to use with the default boot option, but not with the
## alternatives
## e.g. defoptions=vga=791 resume=/dev/hda5
# defoptions=quiet splash
defoptions=vga=791
</pre>

Save the file and then reboot.

<h3>If you installed Karmic directly</h3>
You'll be running grub 2, which requires <a href="http://ubuntuforums.org/showpost.php?p=8024427&postcount=17">these instructions</a>. I've reformatted them here for posterity:

<strong>Step one:</strong> In <code>/etc/default/grub</code>, add this line: <pre class="lang:bash decode:1 " >set gfxmode=1024x768</pre>

<strong>Step two:</strong> In <code>/etc/grub.d/00_header</code>, add the highlighted line:
<pre class="lang:bash highlight:3 decode:1 " >
if loadfont <code>make_system_path_relative_to_its_root ${GRUB_FONT_PATH}</code> ; then
  set gfxmode=${GRUB_GFXMODE}
  set gfxpayload=keep
  insmod gfxterm
</pre>

<strong>Step three:</strong> Run <code>update-grub</code>, and reboot.