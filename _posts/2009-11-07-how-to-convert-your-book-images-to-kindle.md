---
ID: 654
post_title: 'How to Convert Your Book&#8217;s Images to Kindle'
author: matthew
post_date: 2009-11-07 12:53:10
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/how-to-convert-your-book-images-to-kindle-654.html
published: true
syntaxhighlighter_encoded:
  - "1"
dsq_thread_id:
  - "161695262"
---
Taking your painstakingly typeset book and shoving it through the kindle "conversion" meatgrinder was an exercise in wincing. Most of the images were corrupted, there was whitespace sprinkled randomly throughout the copy, and it was a general mess.

Kindle supports direct upload of an html version of your book, but there's a lot of finessing you need to do before it all goes smoothly. One of the tasks you'll need to do is convert your book's images to greyscale, and reduce their size to something Kindle-friendly. There are free tools to help you do this if you aren't afraid of the terminal.

<!--more-->

<a href="http://www.imagemagick.org/">Imagemagick</a> is a command-line tool (which is perfect for these sorts of batch operations) that provides image operations that are extremely high-quality.

Kindle's documentation says that the <a href="http://dtpforums.amazon.com/dtpforums/entry.jspa?externalID=191&amp;categoryID=3">images must not be larger than 450 pixels by 550 pixels</a>, and not exceed 64 Kb. I used  (installed via <a href="http://www.macports.org/">macports</a> with <tt>sudo port install imagemagick</tt>) on my mac to churn through the images in my <a href="http://www.apple.com/iwork/pages/">Pages</a> document. (If you saved your book as "Documents/book.pages", if you right-click the document in the finder and choose "Show Package Contents" you'll see the images.) Here's an example script to batch-convert your images:

<pre class="lang:bash decode:1 " >
for i in *.* ; do
  convert $i \
    -channel RGB -fill White -opaque None +channel -alpha Off \
    -colorspace Gray \
    -resize 450x550\&gt; \
    -unsharp 0 -quality 80 \
    ../$(echo $i|sed -e 's/\..*$//g').jpg
done
</pre>

What this does is add a white background, flatten the canvas (so no pixels are transparent), switch to greyscale (as Kindle isn't color), sharpen the image a bit, set the JPEG quality to 80 (high quality), and output the images into the parent directory, encoded in JPEG format. Although line art is better served with PNG or GIF, I found that using the "quality 80" setting kept JPEG artifacts in rendered text to a minimum.