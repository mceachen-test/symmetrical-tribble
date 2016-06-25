---
ID: 1153
post_title: 'HOWTO: Fix git-gui&#8217;s &#8220;Spell checking is unavailable&#8221; dialog'
author: matthew
post_date: 2011-05-21 12:27:06
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/howto-fix-git-guis-spell-checking-is-unavailable-dialog-1153.html
published: true
dsq_thread_id:
  - "310012494"
---
Launching `git gui` from the command line, when built with <a href="http://www.macports.org/">MacPorts</a>, causes an irritating dialog to pop up:

<img src="/blog/wp-content/uploads/2011/05/missing-aspell-300x139.png" alt="" title="missing-aspell" width="300" height="139" class="alignnone size-medium wp-image-1163" />

The solution is simple:

<pre>$ sudo port install aspell aspell-dict-en</pre>