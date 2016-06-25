---
ID: 1283
post_title: >
  Inspecting ActiveRecord-generated
  queries
author: matthew
post_date: 2013-05-18 15:08:44
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/inspecting-activerecord-generated-queries-1283.html
published: true
dsq_thread_id:
  - "1297693791"
---
For the vast majority of rails CRUD applications, you aren't going to need exotic SQL queries, but when you need to make assertions on the generated query, there are two options at your disposal.

Assume you have the following model:

<pre>
class Post
  belongs_to :author
  has_many :comments
  scope :publicly_visible, -> { where(public: true) }
end
</pre>

<!--more-->

For <code>scopes</code> and <code>associations</code>, like <code>publicly_visible</code> and <code>comments</code>, you can call <code>to_sql</code>:

<pre>
irb(main):001:0> Post.publicly_visible.to_sql
=> "SELECT `posts`.* FROM `posts`  WHERE `posts`.`public` = 1"
</pre>

<pre>
irb(main):001:0> Post.create.comments.to_sql
=> "SELECT `comments`.* FROM `comments`  WHERE `comments`.`post_id` = 1"
</pre>

But there isn't a corresponding method on non-enumerable attributes, like "author."

What to do? Hook into the <code>log</code> call in the connection adapters. Add this to your <code>spec_helper.rb</code> or <code>test_helper.rb</code>:

<pre>
# Add support for fetching the last SQL query from database connections:
module LogLastQuery
  attr_accessor :last_query

  def log(sql, *args)
    self.last_query = sql
    super
  end
end

ActiveRecord::ConnectionAdapters::AbstractAdapter.subclasses.each do |ea|
  ea.send(:include, LogLastQuery)
end
</pre>

<code>ActiveRecord::Base.connection.last_query</code> will contain the last query:

<tt>test/unit/post_test.rb:</tt>
<pre>
require 'test_helper'

class PostTest < ActiveSupport::TestCase
  test "author query is reasonable" do
    author = Author.create
    author.posts.create.author(force_reload = true)
    # this isn't an interesting assertion, of course—only an example:
    assert_include(Post.connection.last_query, "`authors`.`id` = #{author.id}") 
  end
end
</pre>