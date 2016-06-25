---
ID: 786
post_title: >
  Defensibility as an touchstone for
  development decisions
author: matthew
post_date: 2010-02-12 22:50:23
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/defensibility-as-an-touchstone-for-development-decisions-786.html
published: true
syntaxhighlighter_encoded:
  - "1"
dsq_thread_id:
  - "182499586"
---
The topic for this "Software Development Mantras" post is "<strong>defensibility</strong>."

First, let me introduce the concept of "<a href="http://en.wikipedia.org/wiki/There%27s_more_than_one_way_to_do_it">TIMTOWDI</a>" (pronounced "tim-toady"). It's an acronym for "there is more than one way to do it," and software development may be unique in that for any given task, this holds true.

<!--more-->

So, given N different ways of implementing the task at hand, which one is better?

If one way is <strong>dramatically simpler</strong> (either conceptually or implementation-wise), that may be guide your decision process.

If one way is especially <strong>more testable</strong>, that may indicate better separation of concerns, or at least lead to a provably correct solution.

I've found there's a simple litmus test that is always applicable, though. Just put yourself in the developer's shoes several months from now, who needs to work on this codebase, and ask yourself: "which solution is the most obvious, least surprising, and most defensible." It may seem like CYA, but it's for a good cause -- you don't want the engineers of tomorrow putting your code on <a href="http://thedailywtf.com/">thedailywtf</a>!

What other development truisms have you found?