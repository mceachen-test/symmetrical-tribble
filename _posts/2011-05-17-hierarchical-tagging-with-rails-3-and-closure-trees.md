---
ID: 1131
post_title: >
  Hierarchical Tagging with Rails 3 and
  Closure Trees
author: matthew
post_date: 2011-05-17 23:26:15
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/hierarchical-tagging-with-rails-3-and-closure-trees-1131.html
published: true
dsq_thread_id:
  - "306721214"
---
In rebuilding <a href="http://photostructure.com/">PhotoStructure</a> on Rails, I was surprised that one of the most popular gem for trees used a <a href="http://en.wikipedia.org/wiki/Nested_set_model">nested set model</a>. Nested sets are performant for reads, but for adding and deleting nodes, it's <strong>extremely</strong> expensive -- on average, half of the rows for your model acting as a nested set have to be updated for every add or delete. <strong>That's crazy-talk!</strong> It requires a table-level lock for something that, at least for PhotoStructure, is a very common task. To top it off, the gem doesn't even do the lock!

<a href="http://matthew.mceachen.us/blog/wp-content/uploads/2011/05/hier-table-design.png"><img class="alignright size-medium wp-image-1132" title="hier-table-design" src="http://matthew.mceachen.us/blog/wp-content/uploads/2011/05/hier-table-design-300x166.png" alt="" width="300" height="166" /></a>I then found the <a href="https://github.com/stefankroes/ancestry">ancestory</a> gem, which works by materializing the ancestral path as a string and storing that as a column. <a href="http://karwin.blogspot.com/">Bill Karwin's</a> excellent <a href="http://www.slideshare.net/billkarwin/models-for-hierarchical-data">Models for hierarchical data presentation</a> describes this as the Path Enumeration algorithm, which doesn't have referential integrity, and relies on performant LIKE selects.

As the tag hierarchies in PhotoStructure are never moved, closure trees should prove to be ideal. It's an excellent excuse to <a href="http://asciicasts.com/episodes/183-gemcutter-jeweler">learn</a> <a href="http://rubygems.org/pages/gem_docs">how</a> to <a href="http://blog.thepete.net/2010/11/creating-and-publishing-your-first-ruby.html">build and publish</a> a rails plugin gem, so I've built a new gem to support closure trees, and I named it "closure_tree". Check it out at:
<h2 style="text-align: center;"><a href="http://mceachen.github.io/closure_tree/"><strong>http://mceachen.github.io/closure_tree/</strong></a></h2>
<h2 style="text-align: center;"><a href="http://mceachen.github.io/closure_tree/"><strong> </strong></a></h2>
<h3>Update, May 24</h3>
1.0.0.beta1 released. All public methods have test coverage.
<h3>Update, May 25</h3>
1.0.0.beta2 and 1.0.0.beta3 were released, which added <code>find_or_create</code> class and instance methods, ancestry_path, and root instance methods. Documentation is on the README and in the rdocs.
<h3>Update, May 29</h3>
1.0.0.beta5 is released, which cleaned up the ancestor and descendant relationships to use <code>has_and_belongs_to_many</code>, and adds <code>leaves</code> class and instance methods.
<h3>Update, Oct 26</h3>
2.0.0.beta1 is released, which added the :dependant option, switched from an embedded dummy rails app for testing to rspec, and much better test coverage (including tests and fixes for a couple reported issues) under sqlite, MySQL, and PostgreSQL.
<h3>Update, Nov 27</h3>
3.0.0 is released, which supports polymorphic trees.
<h3>Update, June 2014</h3>
BOOM. Lots of stuff since the last update!
<ul>
	<li>support for concurrency, 5 rubies, and all current rails versions</li>
	<li>more than 20 contributing engineers</li>
	<li><span style="font-size: 13px;">65 released versions in total</span></li>
	<li><span style="font-size: 13px;">more than 80,000 downloads</span></li>
</ul>