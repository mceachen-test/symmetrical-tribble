---
ID: 1397
post_title: >
  Give your Raspberry Pi a Shutdown Switch
  For Free
author: matthew
post_date: 2015-08-01 22:05:49
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/give-your-raspberry-pi-a-shutdown-switch-for-free-1397.html
published: true
dsq_thread_id:
  - "3994918353"
---
Tired of your headless Raspberry Pi corrupting itself because it never gets to shut down properly?

If you use a USB wifi dongle, you can use the act of unplugging the dongle to tell your Pi to shut down gracefully. Run `lsusb` and look for your wireless adapter:

<!--more-->

<pre class="mark:2">
pi@raspberrypi ~ $ lsusb
Bus 001 Device 004: ID 148f:5370 Ralink Technology, Corp. RT5370 Wireless Adapter
Bus 001 Device 003: ID 0424:ec00 Standard Microsystems Corp. SMSC9512/9514 Fast Ethernet Adapter
Bus 001 Device 002: ID 0424:9514 Standard Microsystems Corp. 
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
</pre>

Create a new file, <tt>/etc/udev/rules.d/99-dongle-shutdown.rules</tt>, and <strong>change the model and vendor values to match your dongle</strong>. For mine, it looks like this:

<pre>
ACTION=="remove", ENV{ID_VENDOR_ID}=="148f", ENV{ID_MODEL_ID}=="5370", RUN+="/sbin/shutdown -h now"
</pre>

Problems? Read <a href="http://raspberrypi.stackexchange.com/a/4722/26569">this stackexchange comment</a> that I found after I made this post, and <a href="http://www.reactivated.net/writing_udev_rules.html#files">udev manual</a> might also be of assistance.