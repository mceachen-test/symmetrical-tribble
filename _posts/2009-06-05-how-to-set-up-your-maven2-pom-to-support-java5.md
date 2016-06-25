---
ID: 419
post_title: >
  How to set up your Maven2 POM to support
  Java5
author: matthew
post_date: 2009-06-05 16:43:14
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/how-to-set-up-your-maven2-pom-to-support-java5-419.html
published: true
syntaxhighlighter_encoded:
  - "1"
dsq_thread_id:
  - "161109497"
---
Seeing this?
<pre class="lang:bash highlight:3,4 decode:1 " >
$ mvn compile
...
generics are not supported in -source 1.3
(try -source 1.5 to enable generics)
</pre>

You need to tell the maven-compiler-plugin to use java 1.5. Add this to your pom.xml:

<pre class="lang:xml decode:1 " >
&lt;project&gt;
...
  &lt;build&gt;
    &lt;plugins&gt;
      &lt;plugin&gt;
        &lt;groupId&gt;org.apache.maven.plugins&lt;/groupId&gt;
        &lt;artifactId&gt;maven-compiler-plugin&lt;/artifactId&gt;
        &lt;version&gt;2.0.2&lt;/version&gt;
        &lt;configuration&gt;
          &lt;source&gt;1.5&lt;/source&gt;
          &lt;target&gt;1.5&lt;/target&gt;
        &lt;/configuration&gt;
      &lt;/plugin&gt;
    &lt;/plugins&gt;
  &lt;/build&gt;
&lt;/project&gt;
</pre>