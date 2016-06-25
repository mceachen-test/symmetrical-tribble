---
ID: 7
post_title: Friendly Popups
author: matthew
post_date: 2005-01-20 01:02:16
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/friendly-popups-7.html
published: true
syntaxhighlighter_encoded:
  - "1"
btc_comment_counts:
  - 'a:0:{}'
btc_comment_summary:
  - 'a:0:{}'
dsq_thread_id:
  - "168646491"
---
 If you want a link to open a new window, and can't use target="_blank", you can set javascript to the onclick attribute and make the href go to "#", but then you can't ctrl-click the link to open it in a new tab, nor can you see where the link is going in the status bar. Here's a solution inspired by <a href="http://www.quirksmode.org/js/popup.html">quirksmode</a>:

<pre>&lt;a href="/some/link.html" onClick="return pop(this)"&gt;</pre>

Here's the code for pop():

<pre class="lang:javascript decode:1 " >
/*
Inspired by http://www.quirksmode.org/js/popup.html
If you want to override the name of the popup window (NOT the title of
the popup), add a &quot;name&quot; attribute to the link.

If you want to override the window features of the popup, add a
&quot;windowfeatures&quot; attribute to the link.
*/

function pop(link) {
    var windowfeatures = 'scrollbars=yes,top=100,left=200,height=250,width=450';
    if (link.getAttribute('windowfeatures')) windowfeatures = link.getAttribute('windowfeatures');
    name = 'pop';
    if (link.getAttribute('name')) pop = link.getAttribute('name');
    newwindow = window.open(link.href, name, windowfeatures);
    if (window.focus) {
        newwindow.focus();
    }
    return false;
}
</pre>