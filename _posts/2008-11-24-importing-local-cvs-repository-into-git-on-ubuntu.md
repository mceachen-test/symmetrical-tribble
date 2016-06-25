---
ID: 207
post_title: >
  Importing a local CVS repository into
  git on ubuntu
author: matthew
post_date: 2008-11-24 13:42:04
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/importing-local-cvs-repository-into-git-on-ubuntu-207.html
published: true
btc_comment_summary:
  - 'a:0:{}'
btc_comment_counts:
  - 'a:0:{}'
dsq_thread_id:
  - "161509085"
---
<a href="http://toolmantim.com/article/2007/12/5/setting_up_a_new_remote_git_repository">Every</a> <a href="http://scie.nti.st/2007/11/14/hosting-git-repositories-the-easy-and-secure-way">post</a> I found in the googles referenced pserver (CVS's clear-text socket-server protocol) when importing a CVS repository. Until I found <a href="http://maymay.net/blog/2008/04/15/how-to-import-cvs-code-repositories-into-git-using-git-cvsimport/">this</a>. Huzzah.

So I wanted to import a local CVS repository -- something sitting in <tt>/home/cvs</tt>, and setting CVSROOT didn't work. The secret is
<ul>
	<li> to not set the CVSROOT environment variable,</li>
	<li> to use the ":local:" cvs protocol</li>
	<li> to realize that the current working directory will be the destination directory for the new git repo.</li>
</ul>
This is the magick incantation (again, assuming that your CVSROOT lives in /home/cvs, and your cvs module to import is called "<a href="http://photostructure.com">photostructure</a>"):
<pre>mkdir ~/photostructure.git ; cd ~/photostructure.git
git-cvsimport -v -d :local:/home/cvs photostructure</pre>