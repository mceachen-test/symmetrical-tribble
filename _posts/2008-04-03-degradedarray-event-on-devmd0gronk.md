---
ID: 3
post_title: DegradedArray event on /dev/md0:gronk
author: matthew
post_date: 2008-04-03 00:18:56
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/degradedarray-event-on-devmd0gronk-3.html
published: true
btc_comment_counts:
  - 'a:0:{}'
btc_comment_summary:
  - 'a:0:{}'
dsq_thread_id:
  - "177945736"
---
Due to an unscheduled powercycle on my linux server, I got a very troubling page from mdadm, the multi-disk administrator, saying it had marked one of the disks as failed.

This, presumably, was due to a flaky SATA controller that didn't make /dev/sda available by the time the kernel was mounting /dev/md0, so software raid turned it off.

It was easy enough to get the drive back into play:

<pre>sudo mdadm /dev/md0 --add /dev/sda1</pre>

And easy enough to monitor progress:
<pre>
mrm@gronk:~$ cat /proc/mdstat 
Personalities : [raid6] [raid5] [raid4]
md0 : active raid5 sda1[3] dm-2[1] dm-1[0]
580074880 blocks level 5, 64k chunk, algorithm 2 [3/2] [UU_]
 [====&gt;................]  
recovery = 22.0% (63875072/290037440) 
finish=178.1min speed=21154K/sec
</pre>