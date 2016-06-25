---
ID: 40
post_title: 'Dealing with a directory with ~&#8734; files'
author: matthew
post_date: 2008-04-23 16:00:08
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/dealing-with-a-directory-with-tons-of-files-40.html
published: true
btc_comment_counts:
  - 'a:0:{}'
btc_comment_summary:
  - 'a:0:{}'
dsq_thread_id:
  - "170432368"
---
Got a directory with > 10K of files? Need to move them up one directory? <code>mv</code> will fail you:

<pre>
$ mv * ..
-bash: /bin/mv: Argument list too long
</pre>

The solution is to list the files one line at a time (with <tt>find</tt> or <tt>ls -1</tt>) and feed that to <tt>xargs</tt>:

<pre>
ls -1 | head -100 | xargs -I f mv f ..
</pre>

This moves the first 100 files up one directory. 

Note that this won't work if you've got whitespace in your filenames. Use <code>find -0</code> and <code>xargs -0</code> to null-separate your filenames in that case.