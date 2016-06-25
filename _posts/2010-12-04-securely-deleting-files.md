---
ID: 1057
post_title: Securely deleting files
author: matthew
post_date: 2010-12-04 12:19:49
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/securely-deleting-files-1057.html
published: true
dsq_thread_id:
  - "184775184"
---
I needed to switch cellphones, and given that my cell's SD card had sensitive data, just formatting the card wasn't sufficient -- it's trivial to recover files from high-level-formatted FAT file system.

I present to you the world's most dangerous (unix) command:

<pre lang="bash">
 find /path/to/mounted/SDcard -type f -print0 | xargs -0 shred -z -u
</pre>

(shred is part of the <code>coreutils</code> package, so it should be installed already)