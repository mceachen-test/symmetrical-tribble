---
ID: 12
post_title: Tunnelblick crash recovery
author: matthew
post_date: 2008-04-04 23:06:54
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/tunnelblick-crash-recovery-12.html
published: true
btc_comment_summary:
  - 'a:0:{}'
btc_comment_counts:
  - 'a:0:{}'
dsq_thread_id:
  - "162204305"
---
I've found that waking my mac from suspend in a different network than it went to sleep in can crash <a href="http://www.tunnelblick.net/">tunnelblick</a>, or cause tunnelblick to try to spin up another openvpn instance, leaving the network wedged. The workaround is to invoke this in a terminal:

<pre>sudo killall -v openvpn</pre>

then relaunch tunnelblick.

If that doesn't work, force-unloading the kernel extension does the trick:

<pre>sudo killall -v openvpn
sudo killall -v Tunnelblick
sudo kextunload -b foo.tun</pre>

then relaunch tunnelblick.