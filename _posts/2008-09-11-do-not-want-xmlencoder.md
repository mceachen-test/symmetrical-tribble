---
ID: 129
post_title: java.beans.XMLEncoder Considered Harmful
author: matthew
post_date: 2008-09-11 02:13:25
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/do-not-want-xmlencoder-129.html
published: true
keywords:
  - |
    >
        xmlencoder, poor performance, xstream
        versus xmlencoder 
description:
  - >
    Performance from java.beans.XMLEncoder
    is unacceptably poor. Use Xstream
    instead.
btc_comment_counts:
  - 'a:0:{}'
btc_comment_summary:
  - 'a:0:{}'
dsq_thread_id:
  - "160993381"
---
To all java engineers out there:

<img src="http://matthew.mceachen.us/blog/wp-content/uploads/2008/09/do-not-want-xml-encoder.jpg" alt="" title="do-not-want-xml-encoder" width="352" height="400" />

Before you even <strong>think</strong> about using <code>java.beans.XMLEncoder</code> and it's sibling, <code>XMLDecoder</code>, make sure you verify two things:

Are you OK with your application running single-threaded? XMLEncoding hits a static synchronized helper method. Expect to see blocked threads.

Are you OK with forcing your JVM to process an obscene amount of object garbage? A 1 hour stress test generated more than 1.2 Tb of object garbage. While the stress test was running, the JVM was between 50 and 80% devoted to garbage collection.

After switching to <a href="http://xstream.codehaus.org/">xstream</a>, the GC problems are gone, there aren't any blocked threads, my whites are whiter, my coat is a whole lot more glossy, and cherubs are flying by with banners that say "Huzzah."