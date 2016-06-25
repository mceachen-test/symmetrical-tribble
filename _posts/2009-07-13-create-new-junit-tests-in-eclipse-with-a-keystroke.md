---
ID: 469
post_title: >
  Create new junit tests in Eclipse with a
  keystroke
author: matthew
post_date: 2009-07-13 18:22:19
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/create-new-junit-tests-in-eclipse-with-a-keystroke-469.html
published: true
syntaxhighlighter_encoded:
  - "1"
dsq_thread_id:
  - "161267137"
---
The Galileo version of Eclipse doesn't ship with a default keybinding for creating a unit test for the current class, but you can add one easily enough. Navigate to Preferences > General > Keys, and search for "junit test case":

<img class="alignnone size-full wp-image-472" title="eclipse-create-junit-keystroke" src="http://matthew.mceachen.us/blog/wp-content/uploads/2009/07/eclipse-create-junit-keystroke.jpg" alt="eclipse-create-junit-keystroke"/>

Click the "Binding" field, hit the keystroke (I used shift-control-option-command-t), and click Apply.

I haven't found a way to jump to the referring test for a given class, though. IDEA's JUnit plugin had that and I miss it. :-\