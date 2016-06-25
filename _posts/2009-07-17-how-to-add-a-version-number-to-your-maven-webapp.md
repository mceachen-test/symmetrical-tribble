---
ID: 482
post_title: >
  How to add a version number to your
  maven webapp
author: matthew
post_date: 2009-07-17 22:30:24
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/how-to-add-a-version-number-to-your-maven-webapp-482.html
published: true
syntaxhighlighter_encoded:
  - "1"
dsq_thread_id:
  - "161309086"
---
I couldn't find a simple recipe to add a version number to a maven-built webapp. The <a href="http://maven.apache.org/plugins/maven-war-plugin/examples/adding-filtering-webresources.html">maven-war-plugin</a> talks about how to filter, but no simple example is given.

<!--more-->

So. I'll assume you are building your webapp with hudson, that you're using subversion, and that your webapp has some text file(s) (.html or .jsp, perhaps?) in <code>src/main/webapp</code> that are a good place to display your version number.

<ol><li><strong>Add a "display_version" to your maven properties</strong>:
<pre class="lang:xml highlight:8 decode:1 " >
&lt;project ...
 &lt;groupId&gt; ... 
 &lt;artifactId&gt; ...
 &lt;name&gt; ...
 &lt;packaging&gt;war&lt;/packaging&gt;
 &lt;version&gt;1.0-SNAPSHOT&lt;/version&gt;
 &lt;properties&gt;
   &lt;display_version&gt;v${project.version}-${SVN_REVISION}&lt;/display_version&gt;
 &lt;/properties&gt;
</pre>

Note that hudson will fill in the SVN_REVISION value for you. If you want some other number from the build in your version, you can <a href="http://wiki.hudson-ci.org/display/HUDSON/Building+a+software+project#Buildingasoftwareproject-HudsonSetEnvironmentVariables">use other hudson variables</a>, but cook the version number to your taste.
</li>
<li><strong>Configure the maven-war-plugin to enable filtering</strong>:
<pre class="lang:xml highlight:20 decode:1 " >
&lt;project 
  ...
  &lt;build&gt;
    &lt;plugins&gt;
      &lt;plugin&gt;
        &lt;groupId&gt;org.apache.maven.plugins&lt;/groupId&gt;
        &lt;artifactId&gt;maven-war-plugin&lt;/artifactId&gt;
        &lt;version&gt;2.1-alpha-2&lt;/version&gt;
        &lt;configuration&gt;
          &lt;nonFilteredFileExtensions&gt;
            &lt;nonFilteredFileExtension&gt;gif&lt;/nonFilteredFileExtension&gt;
            &lt;nonFilteredFileExtension&gt;ico&lt;/nonFilteredFileExtension&gt;
            &lt;nonFilteredFileExtension&gt;jpg&lt;/nonFilteredFileExtension&gt;
            &lt;nonFilteredFileExtension&gt;png&lt;/nonFilteredFileExtension&gt;
            &lt;nonFilteredFileExtension&gt;pdf&lt;/nonFilteredFileExtension&gt;
          &lt;/nonFilteredFileExtensions&gt;
          &lt;webResources&gt;
            &lt;resource&gt;
              &lt;directory&gt;src/main/webapp&lt;/directory&gt;
              &lt;filtering&gt;true&lt;/filtering&gt;
            &lt;/resource&gt;
          &lt;/webResources&gt;
        &lt;/configuration&gt;
      &lt;/plugin&gt;
    &lt;/plugins&gt;
  &lt;/build&gt;
&lt;/project&gt;
</pre>
(Note that the nonFilteredFileExtension prevents binary file corruption. <a href="http://jira.codehaus.org/browse/MWAR-145">It was added in the maven-war-plugin 2.1-alpha-2</a>.)

</li>
<li>Finally, <strong>Insert <code>${display_version}</code> into your webapp</strong>:
I had a jsp/html file that had a footer with a copyright notice that was a nice place to append a version stamp:

<pre class="lang:html decode:1 " >
...
&lt;div class=&quot;footer&quot;&gt;(c) 2009 All rights reserved. ${display_version}&lt;/div&gt;
</pre>
</li>
</ol>

Run <code>mvn war:exploded</code> or <code>mvn package</code> to try it out.

<h4>Update December 14, 2009:</h4>
Note that Hudson's SVN_REVISION macro is empty if you build your project as a maven multi-module. Use one of the other hudson macros in your display_version instead.