---
ID: 348
post_title: IDEA versus Eclipse
author: matthew
post_date: 2009-02-26 20:28:23
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/idea-versus-eclipse-348.html
published: true
dsq_thread_id:
  - "160993432"
---
<a href="http://www.jetbrains.com/idea/">IDEA</a> has been giving me grief lately. Fed up with mysterious pauses, bugs in svn moves, and crashes even after performing cache-deletion voodoo in IDE, I'm back to trying <a href="http://www.eclipse.org">Eclipse</a>.

Keystroke differences and functionality differences have been the major stumbling blocks. The things I miss from IDEA:
<ul>
	<li>Refactor &gt; Safe delete</li>
	<li>A single keystroke that does svn update for the whole project</li>
	<li>smart-complete that builds anonymous inner classes with the methods stubbed out automatically<em> (I found the secret -- ctrl-space after typing "new", the interface, then left paren)</em></li>
	<li>smart-complete that only offers classes or fields that are semantically correct (rather than eclipse's string-matching-based complete). <em>This is my most frequent grief.</em></li>
	<li>The ability to group together sets of files for committing (but I think Mylyn does this) <em>(Switch to the SVN Repository perspective, and open the Synchronize window)</em></li>
	<li>The ability to rollback or make changes to files while still in the commit window <em>(again, you can surf your pending changes in the SVN Repository's "Synchronize" window)</em></li>
	<li>There are bugs in Refactor &gt; Move -- not all string or spring XML references get fixed properly.</li>
	<li>The code formatting preferences in Eclipse are much less flexible than IDEA's.</li>
	<li>I haven't found the keystroke yet to "go to next error/warning".</li>
	<li>Spring IDE gets confused if the XML file isn't fairly well formed. IDEA seems to still be OK if the file is in tatters.</li>
	<li>ctrl/cmd-clicking targets in ant build files don't take you to the target definition. They take you to a nearby matching string, which may or may not be the target.</li>
</ul>
Things that I found that work better in eclipse:
<ul>
	<li>Eclipse kicks IDEA's booty up and down the street when it comes to performance. Eclipse startup time for my large-ish work project: 19 seconds. IDEA, opening the same project: 2 minutes, 25 seconds. That's after an initial launch and quit, so they both should be using only cached-in-RAM files. Eclipse never pauses while typing. Ever. IDEA momentarily wedges every few seconds.</li>
	<li>Projects in your workspace (IDEA calls them "modules") can be "closed" temporarily when you aren't actively working on them. That saves memory and recompilation time.</li>
	<li>You can run automatic code-cleanup fixes on save (and only apply those fixes on the lines of code that you touched)</li>
	<li>The templates support auto-imports and auto-static-imports (fancy!)</li>
	<li>IDEA has this artificial partitioning of file types in the project tree based on what facets are enabled in a module -- there's no simple filesystem tree widget for me to find, say, the web.xml that lives in ../web/WEB-INF/. IDEA shoves it into a "libraries" root node. Eclipse has both a project explorer and a package explorer that don't do this weirdness.</li>
	<li>Creating new JUnit tests in Eclipse is sane and intuitive. IDEA doesn't have a junit test plugin that worked reliably for me.</li>
</ul>
Things that are a blessing and a curse:
<ul>
	<li>Eclipse has this weird "it's not real until you hit Save," whereas you never click save in IDEA. A bunch of things (incremental compilation and spring validation) only happen after Save All. But you get used to that.</li>
	<li>Eclipse has very flexible window management -- far exceeding IDEA's. You can dock, stack, drag, quick-hide, pile, and generally get very confused or very satisfied (depending on your experience level) with where windows are. Is an editor on the wrong side? Drag the tab where you want it. IDEA's editor tabs on the mac simply don't work -- after N tabs are shown, the other tabs disappear, and you can activate an editor where the tab isn't visible or in focus. So at least Eclipse's tabs seem to be bug free.</li>
	<li>Eclipse update sites that are external to eclipse.org -- I added a couple plugins that destabilized things to where I couldn't bring any window up without an exception being raised. I ended up having to start from a new bare-metal install again, but my project settings were picked up (including the new keymaps) so that wasn't too bad.</li>
</ul>

<em>Updated May 7th with some of the solutions I've come across</em>.