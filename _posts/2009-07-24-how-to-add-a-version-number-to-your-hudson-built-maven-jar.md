---
ID: 519
post_title: >
  How to add a version number to your
  hudson-built maven jar
author: matthew
post_date: 2009-07-24 19:26:43
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/how-to-add-a-version-number-to-your-hudson-built-maven-jar-519.html
published: true
syntaxhighlighter_encoded:
  - "1"
dsq_thread_id:
  - "160993460"
---
<a href="http://maven.apache.org/shared/maven-archiver/#archive">Maven's jar plugin</a> will automatically add a "pom.properties" in your jar's <code>META-INF/maven/$groupId/$artifactId</code> directory. If you build with <a href="https://hudson.dev.java.net/">hudson</a>, there are a number of other values that you might want to include automatically, though, like the SVN revision number that the artifact was built from, and when it was built.

Here's the configuration for the maven-jar-plugin that adds a couple of the more-interesting hudson variables to your module's jar's <code>META-INF/MANIFEST.MF</code>:

<pre class="lang:xml highlight:12,13,14,15 decode:1 " >
&lt;project&gt;
  ...
  &lt;build&gt;
    &lt;plugins&gt;
      &lt;plugin&gt;
        &lt;groupId&gt;org.apache.maven.plugins&lt;/groupId&gt;
        &lt;artifactId&gt;maven-jar-plugin&lt;/artifactId&gt;
        &lt;version&gt;2.2&lt;/version&gt;
        &lt;configuration&gt;
          &lt;archive&gt;
            &lt;manifestEntries&gt;
              &lt;Svn-Revision&gt;${SVN_REVISION}&lt;/Svn-Revision&gt;
              &lt;Build-Tag&gt;${BUILD_TAG}&lt;/Build-Tag&gt;
              &lt;Build-Number&gt;${BUILD_NUMBER}&lt;/Build-Number&gt;
              &lt;Build-Id&gt;${BUILD_ID}&lt;/Build-Id&gt;
            &lt;/manifestEntries&gt;
          &lt;/archive&gt;
        &lt;/configuration&gt;
      &lt;/plugin&gt;

</pre>

The Maven <a href="http://maven.apache.org/guides/mini/guide-manifest.html">Guide to Working with Manifests</a> and <a href="http://maven.apache.org/shared/maven-archiver/">Maven Archiver</a> pages, along with the <a href="http://wiki.hudson-ci.org/display/HUDSON/Building+a+software+project#Buildingasoftwareproject-HudsonSetEnvironmentVariables">other hudson variables</a> may prove helpful.

<h4>Update December 14, 2009:</h4>
Note that Hudson's SVN_REVISION macro is empty if you build your project as a maven multi-module.