---
ID: 307
post_title: java rake db:migrate implementations
author: matthew
post_date: 2009-02-03 01:55:21
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/java-rake-dbmigrate-implementations-307.html
published: true
syntaxhighlighter_encoded:
  - "1"
dsq_thread_id:
  - "160993407"
---
I was surprised that there were so many libraries that do the same thing -- manage change in  your database schema. Here's what I've found so far:
<ul>
	<li><a href="http://www.liquibase.org">Liquibase</a> (migrations are specified in db-generic XML)</li>
	<li><a href="http://migrate4j.sourceforge.net/">migrate4j</a> (migrations must be writted in java, and has "down" and "up" support)</li>
	<li><a href="http://autopatch.sourceforge.net">AutoPatch</a> ("down" and "up", with both .sql and .java support)</li>
	<li> <a href="http://code.google.com/p/dbmigrate/">dbmigrate </a>(no "down" -- only "up", with both .sql and .java support, but no test coverage)</li>
	<li><a href="http://code.google.com/p/c5-db-migration/">c5-db-migration</a> (nice integration with maven)</li>
</ul>
I've used a heavily patched version of dbmigrate on my last project at work, and it's doing what it's supposed to. I've only read documentation for the other projects.

<h3>Update March 2010:</h3>

dbmigrate is abandonware that doesn't have any unit test coverage. I'd suggest looking at <a href="http://code.google.com/p/c5-db-migration/">c5-db-migration</a>.