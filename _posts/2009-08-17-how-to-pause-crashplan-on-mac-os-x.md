---
ID: 548
post_title: How to pause crashplan on Mac OS X
author: matthew
post_date: 2009-08-17 09:39:02
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/how-to-pause-crashplan-on-mac-os-x-548.html
published: true
syntaxhighlighter_encoded:
  - "1"
dsq_thread_id:
  - "160993472"
---
<a href="http://www.crashplan.com">Crashplan</a> on Windows has a taskbar icon that lets you put the backup daemon to sleep, but the Mac OS X port doesn't seem to have this functionality.

You can't just kill the backup daemon process. The OS X "launchd" superviser daemon will see the process go away, and immediately respawn the process. You have to tell launchd to stop the backup daemon by using the <tt>launchctl</tt> command:

<pre>sudo launchctl unload /Library/LaunchDaemons/com.crashplan.engine.plist</pre>

The crashplan daemon will start again after you reboot, or you manually restart it:

<pre>sudo launchctl load /Library/LaunchDaemons/com.crashplan.engine.plist</pre>