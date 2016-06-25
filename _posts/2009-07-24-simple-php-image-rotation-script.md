---
ID: 504
post_title: Simple PHP image rotation script
author: matthew
post_date: 2009-07-24 00:46:22
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/simple-php-image-rotation-script-504.html
published: true
syntaxhighlighter_encoded:
  - "1"
dsq_thread_id:
  - "161342173"
---
This blog has a rotating header -- if you visit <a href="/header/">/header/</a> and bounce on reload, you'll see a series of random images. Here's how it works:

The header is added to a div with the following CSS:

<pre class="lang:css decode:1 " >
#headerimage{
  background: url(&quot;/header/&quot;) center center no-repeat;
}
</pre>

<code>/header/</code> is a directory in <code>~/public_html/</code>. It contains a bunch of similar-sized images, along with the following <code>index.php</code> file:

<pre class="lang:php decode:1 " >
&lt;?php
$pattern = '/\.(png|gif|jpg|jpeg|svg)$/i';
$dh  = opendir('.');
while (false !== ($filename = readdir($dh))) {
  if (!is_dir($filename) &amp;&amp; $filename[0] != '.' &amp;&amp; preg_match($pattern, $filename)) {
    $files[] = $filename;
  }
}
closedir($dh);
$file = $files[rand(0, count($files) - 1)];
header('content-type: '. mime_content_type($file));
readfile($file);
?&gt;
</pre>

It's pretty simple. It
<ol>
	<li>looks for image files in the same directory (lines 2-9)</li>
	<li>chooses one at random (line 10)</li>
	<li>sets the mimetype of the response based on the image's filename suffix (line 11)</li>
	<li>sends out the file's contents (line 12)</li>
</ol>
What's nifty is you can turn this into an ultra-lightweight slideshow by adding one line:

<pre class="lang:php decode:1 " >
header('refresh: 5');
</pre>

after line 11 (the mime_content_type header). There isn't any transition between the image changes, but we'll leave that for next time.