---
ID: 1114
post_title: Multi-value hashes in Ruby
author: matthew
post_date: 2011-03-24 14:26:22
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/multi-value-hashes-in-ruby-1114.html
published: true
dsq_thread_id:
  - "262365745"
dsq_needs_sync:
  - "1"
---
Any non-trivial project quickly finds itself needing data structures that are more exotic than simple arrays or maps. In my quest for multimap nirvana, I first found references where every value-put call would need to be changed to look like this:

<!--more-->

<pre lang="ruby">
> h = {}
> (h[:key] ||= []) << "value 1"
> (h[:key] ||= []) << "value 2"
> puts h 
{:key=>["value 1", "value 2"]}
</pre>

Obviously, not DRY, and painful to look at. I came across <a href="http://railsforum.com/viewtopic.php?id=38201">this post</a> which talks about using <a href="http://www.ruby-doc.org/core-1.8.7/classes/Hash.html#M000470">the hash constructor</a>:

<pre lang="ruby">
> h = Hash.new{|h,k| h[k] = []}
> h[:key] << "value 1"
> h[:key] << "value 2"
> puts h
{:key=>["value 1", "value 2"]}
</pre>

OK, I think I can live with that.

If you want a multi-value hash whose values are never duplicated, use <code>Set.new</code> in the constructor instead of an array.

If you need arbitrary-depth hashes, though, check this out:

<pre lang="ruby">
> h = Hash.new{|h,k| h[k]=Hash.new(&h.default_proc) } 
> h[:a][:b][:c] = "123"
> puts h
{:a=>{:b=>{:c=>"123"}}}
</pre>

The default_proc mechanics are explained very well <a href="http://railsforum.com/viewtopic.php?pid=113682#p113682">here</a>.