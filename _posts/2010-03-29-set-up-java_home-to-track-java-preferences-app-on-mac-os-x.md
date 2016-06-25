---
ID: 847
post_title: >
  Set up JAVA_HOME to track Java
  Preferences.app on Mac OS X
author: matthew
post_date: 2010-03-29 09:40:57
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/set-up-java_home-to-track-java-preferences-app-on-mac-os-x-847.html
published: true
syntaxhighlighter_encoded:
  - "1"
dsq_thread_id:
  - "160993553"
---
<img src="http://matthew.mceachen.us/blog/wp-content/uploads/2010/03/java-preferences.png" alt="/Applications/Utilities/Java Preferences.app" title="java-preferences" width="148" height="90" class="alignright size-full wp-image-850" />Mac OS X's Java Preferences.app has a pane for switching between versions of the JDK, but I just found out from a coworker (thanks, Mike!) that you can make your shell match that preference easily -- just add this to your <code>~.bashrc</code>:

<pre lang="bash">export JAVA_HOME=$(/usr/libexec/java_home)</pre>

If you change your JDK priority preference, you'll need to re-source your ~/.bashrc or just open a new terminal window.