---
ID: 53
post_title: >
  TestNG, test groups, IDEA, and
  @BeforeMethod
author: matthew
post_date: 2008-06-14 21:02:40
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/testng-test-groups-idea-and-beforemethod-53.html
published: true
syntaxhighlighter_encoded:
  - "1"
btc_comment_counts:
  - 'a:0:{}'
btc_comment_summary:
  - 'a:0:{}'
dsq_thread_id:
  - "160993304"
---
Say you have

<pre lang="java">
public class SomeTestNG {
@BeforeClass
public void setup() { … }
@Test
public void test1() { … }
@Test
public void test2() { … }
}
</pre>

You'll get the following methods invoked:
<ul>
	<li>setup()</li>
	<li>test1()</li>
	<li>test2()</li>
</ul>
Not surprising.

What happens, though, with this?

<pre lang="java">
public class SomeTestNG {
@BeforeClass
public void setup() { … }
@Test(groups = {"database"})
public void test1() { … }
@Test
public void test2() { … }
}
</pre>

And you run the "database" group?

Does setup() get called?

It'd <strong><em>have</em></strong> to, right?

Well...

<img class="alignnone size-thumbnail wp-image-54" title="fail" src="http://matthew.mceachen.us/blog/wp-content/uploads/2008/06/fail.jpg" alt="Not so much." height="200" />

Not so much.

<code>@BeforeClass</code> has a <code>groups()</code> attribute as well, and it's respected when you run group test suites. If you want it to run before all methods, you need to use the <code>alwaysRun = true</code>:

<pre lang="java">
public class SomeTestNG {
@BeforeClass(alwaysRun = true)
public void setup() { … }
@Test(groups = {"database"})
public void test1() { … }
@Test
public void test2() { … }
}
</pre>

What's a serious humdinger--when you run the test manually in IDEA, you're running the test outside the scope of it's group test suite, so the @BeforeClass runs as expected. Only in ant will the @Before... fail to get called.