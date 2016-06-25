---
ID: 197
post_title: Installing git with MacPorts
author: matthew
post_date: 2008-11-15 10:38:22
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/installing-git-with-macports-197.html
published: true
btc_comment_summary:
  - 'a:0:{}'
btc_comment_counts:
  - 'a:0:{}'
dsq_thread_id:
  - "160993386"
---
The Good: <a href="http://www.macports.org/">MacPorts</a> makes <a href="http://git.or.cz/">git</a>, the new source version control system hotness, available to Mac users.

The Bad: MacPorts sometimes has attitude, and poops out trying to compile or install packages.

The Ugly: MacPorts doesn't tell you how to fix it.

So here was what I saw:
<pre highlight="false">$ port search git
cogito     devel/cogito   0.18.2   Git core and cogito tools to provide a fully-distributed SCM
git-core   devel/git-core 1.6.0.4  A fast version control system
qgit       devel/qgit     2.2      A graphical interface to git repositories
stgit      devel/stgit    0.14.3   Push/pop utility on top of GIT
cgit       www/cgit       0.8      A fast web interface for the git source code management system</pre>

Great. I'll install git-core. What additional goodies are there?

<pre highlight="false">$ port info git-core
git-core 1.6.0.4, devel/git-core (Variants: universal, doc, gitweb, svn, bash_completion)
http://git.or.cz/

Git is a fast, scalable, distributed open source version control system focusing on speed and efficiency.

Library Dependencies: curl, zlib, openssl, expat, libiconv
Runtime Dependencies: rsync, perl5.8, p5-error
Platforms: darwin

$ sudo port install git-core +svn +doc +bash_completion +gitweb
...
Error: Target org.macports.fetch returned: fetch failed
Error: The following dependencies failed to build: p5-svn-simple subversion-perlbindings subversion p5-term-readkey rsync popt
Error: Status 1 encountered during processing.</pre>

So we're now at the bad and ugly. When ports cops attitude, run these two commands:

<pre highlight="false">sudo port selfupdate
sudo port upgrade outdated</pre>

and wait. If there are any failures, look at the package that failed, and restart the installation of that package. Once that's completed, re-do the original <code>port install git ...</code> command.