---
ID: 5
post_title: Recursive sort-by-modification-time
author: matthew
post_date: 2004-10-04 00:43:33
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/recursive-sort-by-modification-time-5.html
published: true
btc_comment_counts:
  - 'a:0:{}'
btc_comment_summary:
  - 'a:0:{}'
dsq_thread_id:
  - "160993257"
---
This certainly isn't rocket science, but it also is certainly not something you want to type more than once.

<pre>find . -type f -printf '%T@\t%p\n' | sort -n | cut -f2</pre>

And an application using feh:

<pre>find . -type f -printf '%T@\t%p\n' | sort -n | cut -f2 | xargs feh -F</pre>

And if you want the newest-written-to .log files, searching from the current working directory:

<pre>find . -name \*.log -type f -printf '%T@\t%p\n' | sort -rn | cut -f2 | head -30 | xargs -n 1 ls -lh</pre>