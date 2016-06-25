---
ID: 371
post_title: >
  Greasemonkey script to disable emoticons
  in Jira
author: matthew
post_date: 2009-04-20 22:06:51
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/greasemonkey-script-to-disable-emoticons-in-jira-371.html
published: true
dsq_thread_id:
  - "160993451"
---
If you're using Jira, you've seen it take your SQL and mangle it into emoticon nonsense:

<img class="alignnone size-full wp-image-373" title="jira-emoticon" src="/blog/wp-content/uploads/2009/04/jira-emoticon.png" alt="jira-emoticon" width="208" height="25" />

First, make sure you have <a href="https://addons.mozilla.org/en-us/firefox/addon/greasemonkey/">GreaseMonkey</a> (for Firefox) or <a href="https://chrome.google.com/webstore/detail/tampermonkey/dhdgffkkebhmkfjojejmpbldmpobfkfo?hl=en">Tampermonkey</a> (for Chrome) installed.

Then <a href="/geek/jira_demoticon.user.js">click this link to install the userscript</a> that undoes this madness. <b>Like all userscripts, scroll through and read it before installing!</b> It is enabled for all urls that match "jira" (you may need to change this to match your installation). It walks through the DOM looking for img elements with the 'emoticon' class, and replaces them with a text node. May your journey be free from extraneous gold stars, forevermore.