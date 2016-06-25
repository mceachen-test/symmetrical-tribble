---
ID: 1103
post_title: >
  Run a RAID6 on Amazon EBS For Fun and
  Profit
author: matthew
post_date: 2011-03-18 18:55:07
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/run-a-raid6-on-amazon-ebs-for-fun-and-profit-1103.html
published: true
dsq_thread_id:
  - "257812099"
---
With all this badmouthing Amazon's <a href="http://aws.amazon.com/ebs/">Elastic Block Store</a>, I wanted to share how we've set up our MySQL server for <a href="http://adgrok.com">AdGrok</a>.

<h2>Step 1: Create a bajillion tiny EBS volumes</h2>

We've got a bunch of performance data in MySQL, so our database is in the tens-of-gigabytes size. I didn't want to worry about rebuilding the RAID, so I used the AWS console to build 8 15GiB volumes.

Why eight volumes? Because I wanted to use RAID6.

<!--more-->

<h3>What? RAID6? Wazzat?</h3>

<a href="http://en.wikipedia.org/wiki/Standard_RAID_levels#RAID_6">RAID6</a> lets you loose two volumes in your array before you have data loss, and you can recover transparently (especially when your individual volumes aren't ginormous). By adding 8 volumes into the array, I have 2 hot spares that will be used in case of individual EBS volume failure. Performance (at least according to bonnie++) was on par with RAID5.

<h2>Step 2: Assemble the array</h2>

This is easy with mdadm tools, which you get with <code>sudo apt-get install mdadm</code> on debian/ubuntu.

If you use the AWS console to mount the EBS volumes to, say, <code>/dev/sdd1</code> through <code>/dev/sdd8</code>,  just run

<pre>sudo mdadm -v --create --raid-devices=6 \
  --spare-devices=2 --level=raid6 /dev/md0 /dev/sdd*</pre>

This array is for the data directory for MySQL. While I was at it, I created a separate RAID1 (with 1 spare volume) for MySQL's binlog. I tried building the RAID with a 128 KiB and 256 KiB chunk size, but the default chunk size of 64 KiB proved to be (dramatically) faster for our database (which is using InnoDB with the <code>innodb_file_per_table</code> setting enabled)

<h2>Step 3: Make the RAID array mount on reboot</h2>

Run this: <code>sudo mdadm --detail --scan</code> and remove the

Take those lines, remove the  "metadata" attribute, and paste them into <code>/etc/mdadm/mdadm.conf</code>, after the "DEVICE paritions" line. Mine looks like this:

<pre>
# mdadm.conf
#
# Please refer to mdadm.conf(5) for information about this file.
#

# by default, scan all partitions (/proc/partitions) for MD superblocks.
# alternatively, specify devices to scan, using wildcards if desired.
DEVICE partitions

ARRAY /dev/md0 level=raid6 num-devices=6 spares=1 UUID=c6a66ce3:4e5344e8:09530215:95c2af9a
...
</pre>

<h2>Step 4: Format and mount the array</h2>

Use whatever your local unix hacker says to use. I played it safe and used ext4, although I've used reiserfs for MySQL too in the past with success. 
 
<pre>sudo mkfs.ext4 -b 4096 -R stride=8 /dev/md0</pre> 

Again, you might want to test different block and stride values.

I then added this to /etc/fstab:

<pre>/dev/md0 /db/data ext4 defaults,noatime,nodiratime 0 0</pre>

and created and mounted the array:

<pre>
sudo mkdir -p /db/data
sudo mount /db/data
</pre>

<h2>Step 5: Set up MySQL</h2>

Shut down MySQL, reconfigure the "datadir" parameter in <code>/etc/mysql/my.cnf</code> to be "/db/data/mysql", and copy <code>/var/lib/mysql</code> to <code>/db/data/mysql</code>.

MySQL should start up and you'll be running off RAID6.

Good job! You totally deserve a raise.