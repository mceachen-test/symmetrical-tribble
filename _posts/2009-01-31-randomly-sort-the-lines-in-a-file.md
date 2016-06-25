---
ID: 299
post_title: Randomly sort the lines in a file
author: matthew
post_date: 2009-01-31 14:49:17
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/randomly-sort-the-lines-in-a-file-299.html
published: true
dsq_thread_id:
  - "160993403"
---
There are times when you need to randomly order the lines in a file. Here's a script to do just that:
[code='perl']
#!/usr/bin/perl -w

use strict;
use List::Util 'shuffle';

my @lines = <>;
print shuffle( @lines ); 
[/code]