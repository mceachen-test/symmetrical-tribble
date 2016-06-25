---
ID: 1071
post_title: 'HOWTO: Force https with Amazon Elastic Load Balancer and Apache'
author: matthew
post_date: 2011-03-02 00:10:22
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/howto-force-https-with-amazon-elastic-load-balancer-and-apache-1071.html
published: true
dsq_thread_id:
  - "243419410"
---
The Amazon ELB service now supports https, which is great, but how do you configure Apache such that it redirects all insecure requests to use a secure connection?

It turns out that the ELB adds a <code>X-Forwarded-Proto</code> header that you can capture with a mod_rewrite rule. Here's the configuration snippet:

<pre lang="xml">
<VirtualHost *:80>
  ...
  RewriteEngine On
  RewriteCond %{HTTP:X-Forwarded-Proto} !https
  RewriteRule !/status https://%{SERVER_NAME}%{REQUEST_URI} [L,R]
</VirtualHost>
</pre>

(this assumes your health check is /status, which doesn't require https)