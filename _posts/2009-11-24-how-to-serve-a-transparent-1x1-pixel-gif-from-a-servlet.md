---
ID: 711
post_title: 'How to serve a transparent 1&#215;1 pixel GIF from a servlet'
author: matthew
post_date: 2009-11-24 20:21:19
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/how-to-serve-a-transparent-1x1-pixel-gif-from-a-servlet-711.html
published: true
syntaxhighlighter_encoded:
  - "1"
dsq_thread_id:
  - "160993520"
---
The first issue was how to build the smallest possible byte array that represents a 1x1 GIF. Using ImageMagick piped to base64 made it easy to embed into java code:

<pre lang="bash">
convert -size 1x1 xc:transparent gif:- | base64
</pre>

At servlet load time, un-base64 the gif back into the byte array:

<pre lang="java">
import org.apache.commons.codec.binary.Base64;
...
private static final String PIXEL_B64  = "R0lGODlhAQABAPAAAAAAAAAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw==";
private static final byte[] PIXEL_BYTES = Base64.decode(PIXEL_B64.getBytes());
</pre>

in your handle method, then write those bytes to the output stream:

<pre lang="java">
httpServletResponse.setContentType("image/gif");
httpServletResponse.getOutputStream().write(PIXEL_BYTES);
</pre>