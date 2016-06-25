---
ID: 611
post_title: >
  Automatic disposable email addresses for
  Google Apps
author: matthew
post_date: 2009-08-29 14:49:56
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/automatic-disposable-email-addresses-for-google-apps-611.html
published: true
syntaxhighlighter_encoded:
  - "1"
dsq_thread_id:
  - "160993478"
---
<h3>Plus Suffixing</h3>
There's a pretty nifty feature with gmail that most people don't know about. I've seen people reference it as the "plus hack" or "plus suffixing". If you have <code>example@gmail.com</code>, you can add a "<code><strong>+</strong></code>" followed by any letters and numbers, and it will still be delivered to <code>example@gmail.com</code>.

I've been using this for corporate email forms -- if facebook wants an email, they get <code>matthew+facebook</code>, for example. I can then add a precise filter on that "To:" address and delete all email from that company if it "goes evil."

There's a glitch in plus suffixing, though -- most web forms don't realize that the plus sign is a totally valid email character, and will reject your plus-suffixed hack.
<h3>So what to do?</h3>
Well, if you're using gmail, you're out of luck. But if you're using a google apps account for a custom domain (say, your company or family), and you are an administrator for that domain, you can enable "minus suffixing"!

<!--more-->

<h3>Router bot to the rescue</h3>
Note that you must be an administrator of your google apps account to make this work.

Log into your google apps dashboard. If you're in your email account, there will be a "Manage this domain" link that will take you to the dashboard.
<h4><img class="alignright size-full wp-image-613" title="Picture 8" src="http://matthew.mceachen.us/blog/wp-content/uploads/2009/08/Picture-8.png" alt="Picture 8" width="173" height="75" />First create a "router bot" account.</h4>
<ol>
	<li> Click "<strong>Users and Groups</strong>"</li>
	<li>Click "<strong>Create a new user</strong>"</li>
	<li>Enter "Router" for first name,</li>
	<li>"Bot" for last name</li>
	<li>"router" for the username</li>
	<li>Copy the temporary password for later</li>
	<li>Click the "<strong>create new user</strong>" button
<img class="alignright size-full wp-image-615" title="Picture 11" src="http://matthew.mceachen.us/blog/wp-content/uploads/2009/08/Picture-11.png" alt="Picture 11" width="331" height="70" /></li>
</ol>
<h4>Tell "Catch-all" to use the router bot.</h4>
<ol>
	<li> Click "<strong>Service Settings</strong>" then "<strong>Email</strong>"</li>
	<li>In the "<strong>Catch-all address</strong>" section, choose "<strong>Forward the email to:</strong>" and enter "<strong>router</strong>"</li>
	<li>Click "<strong>Save changes</strong>" at the bottom of the page</li>
</ol>
<h4>Configure the router bot</h4>
<ol>
	<li>Log into the "router" account's email</li>
	<li>Click "<strong>Settings</strong>"</li>
	<li>Click "<strong>Filters</strong>"</li>
	<li>Then for each user you want to enable routing for, do these steps:
<ol type="a">
	<li>Let's say the username is <code>bob</code></li>
	<li>Click "<strong>Create a new filter</strong>"</li>
	<li>For the user you want to enable routing for, enter the username ("bob") , followed by a dash ("-"), in the "<strong>To:</strong>" column</li>
	<li>Click "<strong>next step</strong>"</li>
	<li>Check "<strong>Forward it to:</strong>" and enter the user's full email address (like "bob@domain.com")</li>
	<li>Check "<strong>Delete it</strong>" as well.</li>
	<li>Click "<strong>Create Filter</strong>"</li>
</ol>
</li>
</ol>
<h4>Test it out</h4>
Sending an email to "bob-test@example.com" should be delivered to "bob@example.com". 

"Dot-suffixing" (bob.test@example.com) should automatically get delivered to bob@example.com). If google changes this routing implementation detail, though, just add another filter to the routerbot that looks for "To: bob." and forwards to bob.