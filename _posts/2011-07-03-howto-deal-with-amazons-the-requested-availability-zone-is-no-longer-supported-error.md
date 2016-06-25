---
ID: 1193
post_title: 'How to deal with Amazon&#8217;s &#8220;requested Availability Zone is no longer supported&#8221; error'
author: matthew
post_date: 2011-07-03 13:27:14
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/howto-deal-with-amazons-the-requested-availability-zone-is-no-longer-supported-error-1193.html
published: true
dsq_thread_id:
  - "349524867"
---
In shutting down the AdGrok servers (talk about bittersweet...), I stopped the instances, but then remembered I wanted to shred the files first, so I clicked "start," and was greeted by the following error:

<p style="color: red;">The requested Availability Zone is no longer supported. Please retry your request by not specifying an Availability Zone or choosing us-west-1b, us-west-1c.</p>

<!--more-->

Assuming your instance was backed by an EBS volume (and there wouldn't be any valuable state for the instance otherwise, so that should be a reasonable assumption), you'll need to migrate your EBS volume to a different availability zone, and start a new instance there.

To move an EBS volume from one availability zone to another, you need to:
<ol>
	<li>create a snapshot of the EBS volume</li>
	<li>use the snapshot to create a new EBS volume in the destination zone</li>
	<li>attach the new EBS volume to an instance in the destination zone</li>
</ol>

You can do all these tasks through the AWS Management Console, in the "ELASTIC BLOCK STORE" sections.