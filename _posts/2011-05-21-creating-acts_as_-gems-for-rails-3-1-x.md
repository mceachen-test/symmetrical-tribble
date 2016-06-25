---
ID: 1146
post_title: 'Creating acts_as_&#8230; gems for Rails 3.1.x'
author: matthew
post_date: 2011-05-21 12:03:38
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/creating-acts_as_-gems-for-rails-3-1-x-1146.html
published: true
dsq_thread_id:
  - "309996633"
---
<strong>Update January 2012:</strong> since originally writing this post, I've recanted my love for rails plugins -- having a stubbed rails app in your plugin's test directory is just too funky. If you need an example of a rails 3.2.x plugin that uses rspec 2 and has Travis CI integrated, check out <a href="https://github.com/mceachen/closure_tree">closure_tree</a>. But even that setup feels to heavyweight.

The original post follows.


There are a <strong>lot</strong> of posts on how to build rubygems. With Rails 3.1, though, they're all <a href="http://www.imdb.com/title/tt0120912/quotes?qt0431193">old and busted</a>.

The new hotness is built right into rails now. Just incant

<pre lang="ruby">rails plugin new APP_PATH</pre>

You'll get:
<ul>
	<li>a .gemspec and Gemfile to get started</li>
	<li>a dummy rails app to integration test your new gem against</li>
	<li>a (deprecated) RDoc rake task</li>
</ul>

When you run <code>rake</code>, you'll see "rake/rdoctask is deprecated.  Use rdoc/task instead (in RDoc 2.4.2+)". To remedy, <a href="http://matthew.mceachen.us/blog/?p=1169">follow these steps</a>.

The <a href="http://edgeguides.rubyonrails.org/plugins.html#add-an-acts_as-method-to-active-record">Edge edition of the Ruby on Rails Guides</a> has more information.