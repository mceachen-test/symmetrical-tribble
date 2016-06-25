---
ID: 346
post_title: Simple Log4J eclipse template
author: matthew
post_date: 2009-02-26 14:15:45
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/simple-log4j-eclipse-template-346.html
published: true
dsq_thread_id:
  - "160993421"
---
Do you use eclipse and log4j? Do you have a template to add a static Logger instance in classes? Do you have to manually add the import? HA! NO MORE!

Under Preferences > Java > Editor > Templates, click New...

Give the template a name (like "logger").

Use this for the Pattern:

<pre>
${:import(org.apache.log4j.Logger)}
private static final Logger LOG = Logger.getLogger(${enclosing_type}.class);
</pre>

Click OK, OK, then incant the template by typing the name of the template and invoke Content Assist (ctrl-space on most platforms).

The import line will add org.apache.log4j.Logger to your imports automatically if it doesn't exist already. Nifty, eh?