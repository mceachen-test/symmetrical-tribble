---
ID: 1089
post_title: >
  Left outer joining a
  has_and_belongs_to_many in a Rails 3
  scope
author: matthew
post_date: 2011-03-16 17:06:30
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/left-outer-joining-a-has_and_belongs_to_many-in-a-rails-3-scope-1089.html
published: true
dsq_thread_id:
  - "255950035"
---
Winner for esoteric rails 3 question, but the googles (and stack overflow and the rails docs) all failed me.

Here's the pretty simple scope syntax for finding posts with or without tags. Pretty, eh?

<!--more-->

<pre lang="ruby">
class Tag < ActiveRecord::Base
  has_and_belongs_to_many :posts
end

class Post < ActiveRecord::Base
  has_and_belongs_to_many :tags
  scope :tagged, lambda do
    includes(:tags).where("tags.id is not null")
  end
  scope :untagged, lambda do
    includes(:tags).where("tags.id is null")
  end
end
</pre>

I found that <code>.to_sql</code> lies, though:

<pre lang="ruby">
> Post.tagged.to_sql
 => "SELECT \"posts\".* FROM \"posts\" WHERE (tags.id is not null)" 
</pre>

But the MySQL query log shows that the correct query is being executed:

<pre lang="sql">
SELECT posts.id AS ... 
FROM posts 
  LEFT OUTER JOIN posts_tags ON posts_tags.post_id = posts.id 
  LEFT OUTER JOIN tags ON tags.id = posts_tags.tag_id 
WHERE (tags.id is not null)
</pre>