---
ID: 1220
post_title: >
  How to test your rails application with
  Travis CI on different databases engines
author: matthew
post_date: 2012-04-08 19:27:35
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/how-to-test-your-rails-application-with-travis-ci-on-different-databases-engines-1220.html
published: true
dsq_thread_id:
  - "641752913"
---
<a href="http://travis-ci.org/" target="_blank">Travis CI</a> is an awesome <a href="http://en.wikipedia.org/wiki/Continuous_integration" target="_blank">continuous integration</a> service that's free for open-source projects.

If you'd like to test your app against multiple database engines, it's fairly simple.
For these examples, I'm testing my app against SQLite, MySQL, and PostgreSQL.

<!--more-->

Edit your <code>#{Rails.root}/.travis.yml</code> (and replace <strong>myapp</strong> with your app name):
<pre language="yaml">language: ruby
rvm:
  - 1.9.3
env:
  - DB=sqlite
  - DB=mysql
  - DB=postgresql
script:
  - RAILS_ENV=test bundle exec rake --trace db:migrate test
before_script:
  - mysql -e 'create database myapp_test'
  - psql -c 'create database myapp_test' -U postgres</pre>
And your <code>config/database.yml</code>:
<pre language="yaml">sqlite: &sqlite
  adapter: sqlite3
  database: db/<%= Rails.env %>.sqlite3

mysql: &mysql
  adapter: mysql2
  username: root
  password:
  database: myapp_<%= Rails.env %>

postgresql: &postgresql
  adapter: postgresql
  username: postgres
  password:
  database: myapp_<%= Rails.env %>
  min_messages: ERROR

defaults: &defaults
  pool: 5
  timeout: 5000
  host: localhost
  <<: *<%= ENV['DB'] || "postgresql" %>

development:
  <<: *defaults

test:
  <<: *defaults

production:
  <<: *defaults
  # TODO: Add erb-echo of credentials</pre>
<h2>How does this work?</h2>
The only tricky line is this:
<pre language="yaml">  <<: *<%= ENV['DB'] || "postgresql" %></pre>
The database.yml is pulled through the ERB interpreter first, and then parsed as YAML. The DB default is "postgresql" (which you could change, of course), and is overridden by the DB environment variable. The only glitch to this approach is if you use a database engine that doesn't have a section defined (like "oracle"), you get a fairly cryptic error message from ActiveRecord that the "database driver must be specified".
<h2>Do you have an example I can look at?</h2>
Certainly! <a href="http://github.com/mceachen/chromotype">Chromotype</a> is an example Rails application that is using this setup successfully (along with Minitest!).