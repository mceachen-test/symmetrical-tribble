---
ID: 801
post_title: Simple spring integration testing
author: matthew
post_date: 2010-02-18 20:43:33
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/simple-spring-integration-testing-801.html
published: true
syntaxhighlighter_encoded:
  - "1"
dsq_thread_id:
  - "162286865"
---
Spring has had a really nice unit test framework available for a while now, but the <a href="http://static.springsource.org/spring/docs/2.5.6/reference/testing.html#testcontext-fixture-di">documentation</a> can be a bit daunting. Here's a super-simple example of adding dependency injection to an integration test:

<!--more-->

Here's <tt>.../src/main/resources/spring.xml</tt>:

<pre lang="xml">
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

  <bean id="bool" class="java.util.concurrent.atomic.AtomicBoolean">
    <constructor-arg value="true"/>
  </bean>
</beans>
</pre>

And the <tt>.../src/test/java/.../ExampleTest.java</tt>:

<pre lang="java">
import java.util.concurrent.atomic.AtomicBoolean;
import javax.annotation.Resource;
import org.junit.Assert;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = {"/spring.xml"})
public class ExampleTest {
    @Resource(name = "bool")
    private AtomicBoolean bool;

    @Test
    public void testBool() throws Exception {
        Assert.assertTrue(bool.get());
    }
}
</pre>

Of course, the point would be to spin up hibernate or whatever other beans you have, rather than spending 50 lines to create a boolean reference.

Here's the maven dependency (assuming you're still on the 2.5.x branch)
<pre lang="xml">
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-test</artifactId>
      <version>2.5.6.SEC01</version>
      <scope>test</scope>
    </dependency>
</pre>