---
ID: 818
post_title: Deliberate Change Management
author: matthew
post_date: 2010-03-06 11:36:50
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/deliberate-change-management-818.html
published: true
syntaxhighlighter_encoded:
  - "1"
dsq_thread_id:
  - "166135437"
---
Software engineering can be described as the orchestration of a quasi-<a href="http://en.wikipedia.org/wiki/Countable_set">denumerable</a> set of moving parts. 

With so many moving parts, breakages occur. One main goal as a "software craftsperson" is to never expose customers to the affects of these breakages. <a href="http://en.wikipedia.org/wiki/Test-driven_development">Test-driven development</a> rose from this desire. Accepting that systems break, even in production, motivated <a href="http://en.wikipedia.org/wiki/Loose_coupling">loosely coupled</a> and <a href="http://en.wikipedia.org/wiki/Shared_nothing_architecture">shared-nothing</a> system architectures.

<!--more-->

Breakages don't just happen in production, of course -- more often than not, an engineer's first implementation won't be completely correct, and will have some sort of defect. When these defects are found, it's critical to know what to blame for the defect. Was it their code? Is the database or some other external service down?  Was it a library? <a href="http://en.wikipedia.org/wiki/Post_hoc_ergo_propter_hoc">Post hoc ergo propter hoc</a>! The last thing I changed must be what broke it! <i>But what if more than one thing changed?</i> A mentor of mine many years ago gave me the following mantra when debugging a broken system:

<blockquote><strong>Only change one thing at a time.</strong> 
-- kjk</blockquote>

If you touch more than one thing between compile/test cycles, and the defect is addressed, you won't know <strong>which</strong> change fixed the issue. This can lead to perverse <a href="http://en.wikipedia.org/wiki/Cargo-cult">cargo-cult</a> practices ("oh, don't do that, because it broke for me once, but I don't know why").

This principle can be summarized as <strong>deliberate change management</strong>. If multiple engineers are touching the same system, it can be frustratingly common for non-communicative teams to break each-other's development environments. The only solutions are to communicate better (either with always-on Skype or physical proximity), or to separate the code into versioned modules (which <a href="http://maven.apache.org/">Maven</a> makes simple).

Proving that a bug exists by building a unit or integration test that fails, and then changing the code such that the test passes, is another example of deliberate change management.

This brings me to my last point -- Maven's "<code>LATEST</code>" and "<code>RELEASE</code>" meta-versions. They may seem like a good thing, but they completely violate deliberate change management. The system that was working just 5 minutes ago is now broken. Was it your module, or did you just upgrade to a newer SNAPSHOT version of some module that you depend on? If you had used the <a href="http://mojo.codehaus.org/versions-maven-plugin/">versions plugin</a> to <strong>deliberately</strong> upgrade your system dependencies, rather than floating on LATEST, you'd <strong>know</strong> it was the library that broke you.