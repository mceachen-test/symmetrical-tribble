---
ID: 1315
post_title: >
  How to make breaking changes and not
  break all the things
author: matthew
post_date: 2014-06-19 01:31:53
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/how-to-make-breaking-changes-and-not-break-all-the-things-1315.html
published: true
dsq_thread_id:
  - "2777407028"
---
<h2>The Problem</h2>
Incremental feature development against existing systems frequently involves rethinking and rebuilding existing components. If it's possible, the easiest way to add support for your new feature is to introduce a new, optional attribute on a model (or in the case of a relational database, adding a NULLable column to an existing table, or adding a new table). Sometimes, though, that either isn't possible, or wouldn't be the best design going forward, given your new knowledge of the feature set of the product at hand. You're left with the uncomfortable realization that the best design would be something completely different <em>that isn't backward-compatible with current systems</em>.

When there are multiple systems that consume a given data model (say, a front-end webserver, an API, tons of cron jobs, and many backend daemons, like in the case of Twitter's Ads systems), you can't just "stop the world," ALTER your tables, and deploy all the new code. That's fine if it's you and another nerd in a garage, but when you've got paying customers, you don't want to give them an excuse to spend their money elsewhere.
<h2>What to do?</h2>
<!--more-->

If you need to make a backward-incompatible change, and all you can do is make backward-compatible changes, then <strong>make<i> several</i> backward-compatible changes.</strong>

In the interests of clarity, let's go through a concrete example. Let's assume we're building a blogging platform (using MySQL), and you've already got a <tt>posts</tt> table:
<pre class="lang:mysql decode:true">CREATE TABLE posts (
  id int NOT NULL AUTO_INCREMENT,
  content text,
  author_id int NOT NULL,
  PRIMARY KEY (`id`)
);
</pre>
We want to support zero or more authors for a given post, and the final design we want to migrate to looks like this:
<pre class="lang:mysql decode:true">CREATE TABLE posts (
  id int NOT NULL AUTO_INCREMENT,
  content text,
  PRIMARY KEY (`id`)
);

CREATE TABLE post_authors (
 post_id int NOT NULL,
 author_id int NOT NULL
);
</pre>

<h3>Step 0: Start from a safe place</h3>
All bets are off if you don't have integration tests in place to prove that with each step you take, you aren't breaking things.

<h3>Step 1: In with the new</h3>
Create the new column or table, but if optionality is required to maintain backward compatibility, then let it be <tt>NULL</tt>able.

In this case, we'd just add the new <tt>post_authors</tt> table as you want it in final form.

<h3>Step 2: Change all write codepaths to double-write from old to new</h3>
Add code to your write paths to write to both the old schema as well as the new schema. Make sure you're writing to the new schema in the same transaction boundary as the old writes—you want rollbacks to keep both sides consistent.

<h3>Step 3: Backfill the old rows</h3>
Once all new writes are persisting to both the old and new schema, write a batch script or SQL update to copy all existing records into the new schema structure, but only where they are missing. See the <a href="http://dev.mysql.com/doc/refman/5.0/en/insert-on-duplicate.html">INSERT IGNORE</a> documentation if you're using MySQL.

<h3>Step 4: Fix any NULLable columns that should be NOT NULL</h3>
If you're adding a new column to an existing table, you probably had to add it as NULLable. Now, thanks to step 3, all existing rows, and, thanks to step 2, all new rows in the new schema are valid, you can ALTER the table to make the column NOT NULL where appropriate.

<h3>Step 5: Change the read codepaths to read from the new</h3>
The new schema design is ready for consumption! Update all the readers to use the new schema. Note that writes must continue to go to the old schema design and the new schema design until all read paths have migrated to the new schema design.

<h3>Step 6: Change the write codepaths to only write to the new</h3>
Now that no reads hit the old schema design, you can update all writers to directly interact with the new schema design, and delete the double-writing code.

<h3>Step 7: Delete the old schema columns or tables</h3>
Once all reads and writes are on the new schema design, the deprecated columns or tables can be dropped.

<h3>Step 8: Whiskey and/or Chocolate</h3>
Apply as appropriate.

<h3>Extra Credit—Use a Fractional Rollout System</h3>
When your system is non-trivially large, or if you just want to reduce risk during launches, do what Twitter, Facebook, Google, and everyone else does—use a fractional-rollout system so you can turn on, say, the new schema readpaths to only 1% of traffic (and immediately roll it back if there are issues). Read more about Twitter's infrastructure <a href="http://www.infoq.com/articles/twitter-infrastructure">here</a> (and search for "Decider").

<a href="https://news.ycombinator.com/item?id=7936056">View the comments on hacker news about this post</a>. 

Updated to reference tests (thanks, Matt and lsh123!) and decider (thanks, jakozaur!).