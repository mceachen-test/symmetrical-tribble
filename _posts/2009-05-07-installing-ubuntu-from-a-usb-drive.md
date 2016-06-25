---
ID: 386
post_title: Installing Ubuntu from a USB Drive
author: matthew
post_date: 2009-05-07 00:36:37
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/installing-ubuntu-from-a-usb-drive-386.html
published: true
dsq_thread_id:
  - "208096608"
---
I just bought a new <a href="http://www.amazon.com/MSI-Wind-Nettop-100-Processor/dp/B001R1X0I0/">green server</a> (25-30 watts at full load!) and wanted a brand-new <a href="http://www.ubuntu.com/">Jaunty</a> experience.

The MSI Wind Nettop doesn't come with an optical disk drive, nor does it come with IDE support, so my old CDROM drives wouldn't work -- I needed to boot from an old 1gb USB drive.

I found <a href="http://en.wikipedia.org/wiki/Ubuntu_Live_USB_creator">usb-creator</a> was already available on my to-be-decommissioned Ubuntu Gutsy box, but it won't work unless you clear off enough free space beforehand. I ended up repartitioning my USB drive with cfdisk, then running mkfs against the new partition, then restarting usb-creator.

The actual installation was quite smooth and quick. Thanks to the development team!