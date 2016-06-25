---
ID: 920
post_title: 'HOWTO: Mount your USB hard drives at boot time on Ubuntu'
author: matthew
post_date: 2010-08-14 21:02:11
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/howto-mount-your-usb-hard-drives-at-boot-time-on-ubuntu-920.html
published: true
dsq_thread_id:
  - "162652196"
---
I've got a number of external USB hard drives connected to my ubuntu server that need to mount to a predictable directory.

When you log into Gnome, the desktop environment does it's nifty thing and mounts any drive you've got plugged in -- but if the box reboots, the drives won't be mounted until the next person logs into the computer. 

I needed something that happens at boot time to do this task.

<!--more-->

There's a <a href="http://superuser.com/questions/53978/ubuntu-automatically-mount-external-drives-to-media-label-on-boot-without-a-us/175976#175976">superuser</a> post that asks this question, but none of the answers were helpful. The highest-rated post relies on a command that doesn't exist.

So first off, make sure your external drives have a label. The label will be the name of the directory that is the mount point. To edit the label, go to "System > Administration > Disk Utility", find the drive, unmount it, and click "Edit filesystem label." 

Then, as root, edit your <code>/etc/rc.local</code> and add these lines:

<pre lang="bash">
for dev in $(ls -1 /dev/disk/by-label/* | grep -v EFI) ; do
  label=$(basename $dev)
  mkdir -p /media/$label
  $(mount | grep -q /media/$label) || mount $dev /media/$label
done
</pre>

All done!

<b>Update 20130608:</b>
Carl-Erik Kopseng's comment points to a <a href="http://superuser.com/a/64970">much more rigorous solution which also supports clean unmounting</a>, which he's <a href="https://github.com/fatso83/Code-Snippets/tree/master/system-utils/ubuntu/automount">copied into his public snippets repository</a>. 

Alternatives are always great!