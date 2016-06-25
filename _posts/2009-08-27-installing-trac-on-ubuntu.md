---
ID: 602
post_title: Installing Trac on Ubuntu
author: matthew
post_date: 2009-08-27 22:15:43
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/installing-trac-on-ubuntu-602.html
published: true
syntaxhighlighter_encoded:
  - "1"
dsq_thread_id:
  - "161553237"
---
Here's the condensed version, using <a href="http://trac.edgewall.org/wiki/TracInstall">the installation guide</a> for help:

<!--more-->

<h3>Install the software</h3>
<pre>sudo apt-get install python-setuptools python-subversion
sudo easy_install Trac</pre>
<h3>Initialize the Trac project</h3>
We're going to run the standalone trac server just for simplicity. You don't want it running as root (or yourself), but it needs at least read access to the subversion repository. Replace "projectname" with the name of your project.
<pre>sudo -u [some role user]
trac-admin ~/trac-projectname initenv</pre>
trac-admin will ask you some questions.
<h3>Run Trac</h3>
To prevent unwanted viewing or edits to your trac instance, enable apache digest authentication. First make an .htdigest file:
<pre>sudo apt-get install apache2-utils
sudo -u [some role user]
htdigest -c ~/.htdigest mycompany.com [admin username]</pre>
(to add more users later, omit the <tt>-c</tt>).

Then tell trac that the admin username is an admin:
<pre>
trac-admin ~/trac-projectname permission add [admin username] TRAC_ADMIN
</pre>

Then launch the standalone Trac instance (replace projectname and mycompany.com as appropriate):
<pre>sudo -u [some role user]
cd ~
tracd -s --port 8000 \
 --auth=trac-projectname,.htdigest,mycompany.com \
 ~/trac-projectname</pre>

If the server is only visible to a trusted network, you can skip the htpasswd command and the --auth line.
Use -s if you only have one subversion repository.