---
ID: 38
post_title: Verifying file integrity with debsums
author: matthew
post_date: 2008-04-21 09:11:24
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/verifying-file-integrity-with-debsums-38.html
published: true
syntaxhighlighter_encoded:
  - "1"
btc_comment_counts:
  - 'a:0:{}'
btc_comment_summary:
  - 'a:0:{}'
dsq_thread_id:
  - "161934607"
---
After upgrading my Ubuntu server, some security applications grumped about changed contents of some common binaries. 

Just to be safe, I wanted to verify them explicitly with debsums, but debsums looks for package names, not paths to binaries. Here's a script that validates chattr, find, perl, and lsattr--the "-s" option to debsums is "silent", so no news is good news:

<pre class="lang:bash decode:1 " >
for i in /usr/bin/chattr /usr/bin/find /usr/bin/perl /usr/bin/lsattr ; do
  echo $i
  debsums -as $(dpkg -S $i | cut -d':' -f1 | sort -u)
done
</pre>