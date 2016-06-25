---
ID: 620
post_title: >
  Simple Image Slideshow with jQuery and
  PHP
author: matthew
post_date: 2009-09-01 01:11:55
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/simple-image-slideshow-with-jquery-and-php-620.html
published: true
syntaxhighlighter_encoded:
  - "1"
dsq_thread_id:
  - "160993490"
---
I really liked <a href="http://jonraasch.com/blog/a-simple-jquery-slideshow">Jon Raasch's jquery-powered slideshow</a>, but integrating it into a page requires a bunch of steps:
<ol>
	<li>Add the jQuery library</li>
	<li>Add the slideshow javascript function</li>
	<li>Add the CSS with opacity and z-layer set properly</li>
        <li>Upload the images for the slideshow someplace</li>
	<li>Enumerate the images' URLs that you want to cycle through</li>
</ol>
Adding images is tedious (you have to upload AND update the image list), and there's no clean support for multiple slideshows per page. I wanted a process that made integration into other pages trivial.

I decided that an IFRAME to host the slideshow would keep jQuery and the slideshow CSS partitioned away from the rest of the embedding page, and would let adding slideshows to new pages have an easy-to-follow recipe, no matter what was already going on in the existing page. The IFRAME needed the template HTML, and if I used PHP, I could generate the image list for the slideshow automatically from the images that live in the same directory as the PHP script.

<!--more-->

So here are the new instructions, using this IFRAME approach:
<ol>
	<li>Make a directory that will contain the images you want in the slideshow. For this example it's "<code>slideshow-demo</code>"</li>
	<li>Copy <a href="/geek/simple-slideshow/index.txt">this file</a> into the "slideshow-demo"  directory and name it <strong><code>index.php</code></strong></li>
<li>Copy the images for the slideshow into the directory.<strong> 
Remember to resize all the images to the same dimensions.</strong></li>
	<li>On the page you want to embed the slideshow, add <pre class="lang:html decode:1 " >
&lt;iframe src =&quot;slideshow-demo/&quot; width=&quot;300&quot; height=&quot;200&quot; 
  scrolling=&quot;no&quot; frameborder=&quot;0&quot;&gt;&lt;/iframe&gt;</pre>
Replace the width and height to be the common dimension of your images</li>
</ol>
Here's a working demo:
<iframe src="/geek/simple-slideshow/" width="300" height="200" scrolling="no" frameborder="0"></iframe>