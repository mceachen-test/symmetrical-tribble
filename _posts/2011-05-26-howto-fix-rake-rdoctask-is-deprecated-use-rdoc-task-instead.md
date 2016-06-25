---
ID: 1169
post_title: 'How to fix &#8220;rake/rdoctask is deprecated. use rdoc/task instead&#8221;'
author: matthew
post_date: 2011-05-26 09:29:26
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/howto-fix-rake-rdoctask-is-deprecated-use-rdoc-task-instead-1169.html
published: true
dsq_thread_id:
  - "314525653"
---
Seeing this?

<pre>rake/rdoctask is deprecated. Use rdoc/task instead (in RDoc 2.4.2+)</pre>

Edit your <code>Rakefile</code> and change these lines:

<pre lang="ruby">
require 'rake/rdoctask'
Rake::RDocTask.new(:rdoc) do |rdoc|
 ...
</pre>

to look like this:

<pre lang="ruby">
require 'rdoc/task'
RDoc::Task.new do |rdoc|
  ...
</pre>

You may need to add <code>gem 'rdoc'</code> to your <code>Gemfile</code>, too. While you're at it, you might want to add <code>rdoc/</code> to your <code>.gitignore</code>, too