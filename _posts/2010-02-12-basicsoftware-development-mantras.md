---
ID: 790
post_title: Completely Correct DRY Simplicity
author: matthew
post_date: 2010-02-12 20:54:54
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/basicsoftware-development-mantras-790.html
published: true
syntaxhighlighter_encoded:
  - "1"
dsq_thread_id:
  - "193477711"
---
This is the first in (what may be a series) of posts where I'll share some concepts, or mantras, that I've found helpful in my experiences with software development.
The following are some of the lowest hanging fruit. These should be (but aren't necessarily) common sense for all software developers:

<!--more-->

<h3><a href="http://en.wikipedia.org/wiki/Don%27t_repeat_yourself">DRY</a></h3>
Don't Repeat Yourself. In fact, be wary of any code that does essentially the same thing but you've still got multiple implementations. Think about it before you copy and paste. There's a superclass just waiting to be refactored into existence, or a strategy hoping to be extracted.
<h3><a href="http://en.wikipedia.org/wiki/KISS_principle">KISS</a>/<a href="http://en.wikipedia.org/wiki/You_ain%27t_gonna_need_it">YAGNI</a></h3>
Keep it Simple, and You Aren't Going To Need It. Simpler, smaller implementations can be <a href="http://en.wikipedia.org/wiki/Grok">grokked</a> faster. Codebases should be considered "plastic" -- if you don't need extensibility, and it's going to complicate the design, assume that when that need comes, the next developer that<em> </em>will add it sensibly.
<h3><a href="http://www.informit.com/articles/article.aspx?p=1235624&amp;seqNum=6">The Boy Scout Rule</a></h3>
Taken from <a href="http://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882">Clean Code</a>, this is the oath given by every developer on your team: "I promise to leave the tree better than I found it."

Better might be because there are <em>more unit tests</em>. Better might be because you found a way to delete a bunch of code by DRYing it up.

There's a potential issue here that can arise because one developer may think a dramatic shift from the prior design of the code makes things "better," so temper this mantra with consistency brought by "Worse is better."
<h3><a href="http://en.wikipedia.org/wiki/Worse_is_better">Worse is better</a></h3>
Read the linked wikipedia article, but IMHO, a passion for simplicity, consistency, and correctness should be what you look for when you hire people.