---
ID: 1076
post_title: >
  Rails 3.0.5 broke my routes and kicked
  my dog
author: matthew
post_date: 2011-03-02 12:35:19
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/rails-3-0-5-broke-my-routes-and-kicked-my-dog-1076.html
published: true
dsq_thread_id:
  - "243843359"
---
We just upgraded <a href="http://adgrok.com">AdGrok</a> from Rails 3.0.4 to 3.0.5, which was <a href="http://weblog.rubyonrails.org/2011/2/27/rails-3-0-5-has-been-released">released a couple days ago</a>. This is a "patch release," which <a href="http://semver.org">according to the rules</a>, has only backwards compatible bug fixes.

A bunch of our integration tests failed before we pushed to production, and we found out that a pretty big change was introduced: namespaced urls are now prefixed by the namespace, and before 3.0.5 (like, 3.0.4), they weren't.

Here's the simplest example of what's going on:

<!--more-->

<ol>
	<li>Create a new rails app: <code>rails new ouch</code></li>
	<li>Edit the Gemfile to use rails 3.0.4</li>
	<li>Run <code>bundle install</code></li>
	<li>Add this to <code>config/routes.rb</code>:
<pre lang="ruby">
  namespace :products do
    resources :comments do
      get "like" => "products/comments#like", :as => "like"
    end
  end</pre>
</li>
	<li>Run <code>rake routes</code>. You'll see the output (which is so "yesterday"):<pre>
<b>comment_like</b> GET /products/comments/:comment_id/like ...</pre></li>
<li>OK, so the url for that would be "<code>comment_like_url</code>". Now let's upgrade to 3.0.5.</li>
<li>Edit the Gemfile to use rails 3.0.5, and run <code>bundle install</code></li>
<li>Now <code>rake routes</code> says:<pre>
<strong>products_comment_like</strong> GET /products/comments/:comment_id/like ...</pre></li>
</ol>

Is no one using namespaces in their routes? Is this that exotic? 

<h4>Update:</h4>

I wasn't aware of routing with <a href="http://guides.rubyonrails.org/routing.html#adding-member-routes">member blocks</a>. Thanks for the comments showing me the right way to do this!