---
ID: 1445
post_title: 'Escape from Apple&#8217;s Pages'
author: matthew
post_date: 2015-09-26 18:03:01
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/escape-from-apples-pages-1445.html
published: true
dsq_thread_id:
  - "4169271996"
---
<img src="/blog/wp-content/uploads/2015/09/escape-from-pages1.jpg" alt="escape-from-pages" width="181" height="215" class="alignright size-full wp-image-1454" />Apple's never been one for long-lived support of their <a href="https://en.wikipedia.org/wiki/AppleWorks">word processing applications</a>, but they stooped to new lows with their word processing application, <a href="http://www.apple.com/mac/pages/">Pages</a>.

The latest version of Pages, version 5, released in 2013, cannot read documents created from version 3, <a href="https://en.wikipedia.org/wiki/Pages_%28word_processor%29#Version_history">last released in 2008</a>.

<strong>Imagine if all the important papers in your filing cabinet were unreadable after 5 years</strong>. It's dumbfounding.

<!--more-->


When you installed Pages 5, the installer kept Pages 3 still installed. The problem is that the wrong version of the software regularly tries to open a document that you double-click on.

The best solution is to export all your documents to a format that is well-supported. And then uninstall Pages and use something else, like <a href="https://www.libreoffice.org/">LibreOffice</a>.

Open a terminal, and:

<pre class="lang:bash">
mkdir ~/bin
export PATH=$HOME/bin/escape:$PATH
git clone https://github.com/mceachen/escape.git ~/bin/escape
cd ~/Documents # or wherever you have your Pages documents
find $(pwd) -name \*.pages -print0 | xargs -0 -n 1 -- p-to-doc
</pre>

This will recursively create a .docx file next to every Pages file in your Documents directory. 

<a href="https://github.com/mceachen/escape">The code is on github</a>!