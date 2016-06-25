---
ID: 737
post_title: >
  How to bounce an application with
  Applescript
author: matthew
post_date: 2009-11-29 23:12:42
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/how-to-bounce-an-application-with-applescript-737.html
published: true
syntaxhighlighter_encoded:
  - "1"
dsq_thread_id:
  - "166929746"
---
Say, for whatever reason, you want to bounce iphoto once an hour. You can do that with AppleScript and cron.

<!--more-->

Copy this into ~/bin/bounce-iphoto:

<pre class="lang:bash decode:1 " >
#!/usr/bin/osascript
on appIsRunning(appName)
  tell application &quot;System Events&quot; to (name of processes) contains appName
end appIsRunning 

if appIsRunning(&quot;iPhoto&quot;) then
  tell application &quot;iPhoto&quot; to quit
  delay 60
  tell application &quot;iPhoto&quot; to run
end if
</pre>

Then wire it up with cron:

<pre class="lang:bash decode:1 " >
chmod 755 ~/bin/bounce-iphoto
crontab -e
</pre>

and add this line to your crontab (which may be empty):

<pre class="lang:bash decode:1 " >
0 * * * * $HOME/bin/bounce-iphoto
</pre>