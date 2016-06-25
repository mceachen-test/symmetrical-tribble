---
ID: 29
post_title: Installing VMware Player on Ubuntu Gutsy
author: matthew
post_date: 2008-04-20 20:23:50
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/installing-vmware-player-on-ubuntu-gutsy-29.html
published: true
syntaxhighlighter_encoded:
  - "1"
btc_comment_counts:
  - 'a:0:{}'
btc_comment_summary:
  - 'a:0:{}'
dsq_thread_id:
  - "160993263"
---
<strong>Please note that these instructions are for Ubuntu 7.10. Newer versions of Ubuntu can just follow the normal installation instructions at <a href="http://www.vmware.com/download/player/">http://www.vmware.com/download/player/</a>.</strong>

So it turns out that the current release of Ubuntu (at least for another four days), "Gutsy Gibbon",  gave <a href="http://www.vmware.com/products/player/" target="_blank">VMware Player</a> <a href="https://bugs.launchpad.net/ubuntu/gutsy/+source/vmware-player/+bug/151424" target="_blank">the big cold shoulder</a> and removed it as an installable package.

To top it off, the email-capture marketing application that VMware uses is broken, so you can't download vmware-player through their website now. <em>(How is this acceptable? Shame on both companies--eloqua for downtime with something as simple as a form capture, and vmware for not canceling their service)</em>.

Some sleuthing came up with <a href="http://download3.vmware.com/software/vmplayer/VMware-player-2.0.3-80004.i386.tar.gz">the direct URL to the download for VMware Player 2.0.3</a>.

While that downloads, go install the required packages:
<pre>sudo apt-get install \
  build-essential linux-headers-generic \
  linux-headers-$(uname -r)</pre>
Uncompress the archive:
<pre>sudo mkdir -p /opt/vmware-player
sudo chown $(whoami) /opt/vmware-player
cd /opt/vmware-player
tar xvzf ~/Desktop/VMware-player-2.0.3-80004.i386.tar.gz</pre>
Then start the install. I'd recommend <strong>not</strong> putting the binaries into <code>/usr/bin</code> (as this will be a non-.deb installation, and it will make the uninstalling easier):
<pre>sudo ./vmware-install.pl
...
In which directory do you want to install the binary files?
[/usr/bin] <strong>/opt/vmware-player/bin</strong>
...</pre>
From here on out, take the default values.