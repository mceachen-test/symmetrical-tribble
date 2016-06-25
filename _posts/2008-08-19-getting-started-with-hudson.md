---
ID: 117
post_title: Getting started with Hudson
author: matthew
post_date: 2008-08-19 20:44:03
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/getting-started-with-hudson-117.html
published: true
syntaxhighlighter_encoded:
  - "1"
btc_comment_summary:
  - 'a:0:{}'
btc_comment_counts:
  - 'a:0:{}'
dsq_thread_id:
  - "161144627"
---
<a href="http://hudson-ci.org/">Hudson</a> is a slick, stable, and easy to install continuous integration environment. You'll be using it in minutes. Honest.

<!--more-->
<h3>Install Java</h3>
Hudson is a java webapp. Ubuntu comes with Sun's JVM installed by default, so this is a no-op.
<h3>Download and Install Hudson</h3>
<pre class="lang:bash decode:1 " >
mkdir -p ~/.hudson
cd ~/.hudson
wget http://hudson-ci.org/latest/hudson.war
java -jar ~/.hudson/hudson.war --httpPort=9000 &amp;
</pre>

You should see something like this:
<pre>Running from: /Users/mrm/.hudson/hudson.war
[Winstone] - Beginning extraction from war file
...
INFO: Completed initialization</pre>
If you don't, you should <a href="http://wiki.hudson-ci.org/display/HUDSON/Meet+Hudson#MeetHudson-Installation">check the documentation on the hudson wiki</a>.
<h3>Create your first Hudson Job</h3>
Go to <a href="http://localhost:9000">http://localhost:9000</a>, and create a new "job" -- jobs hold the build instructions for a project. They can be built from N build commands and N source code repositories, and have N final artifacts.

There are two main configuration items to concentrate on:
<ol>
	<li><strong>where</strong> hudson should get your code, and</li>
	<li><strong>how</strong> it should do a build.</li>
</ol>
The <strong>where</strong> is configured in the "<strong>Source Code Management</strong>" section. The <strong>how</strong> is configured in the "<strong>Build</strong>" section. You'll want to click "Add build step" and choose the appropriate option.

Most likely you want it to rebuild your project whenever someone commits changes. If you aren't familiar with cron, it might be a bit confusing. "<code>*/5 * * * *</code>" means poll every 5 minutes (whenever the minute value is evenly divisible by 5), like so:

<img title="hudson-scm-poll" src="http://matthew.mceachen.us/blog/wp-content/uploads/2008/08/hudson-scm-poll.png" alt="" />

Kudos to the developers!