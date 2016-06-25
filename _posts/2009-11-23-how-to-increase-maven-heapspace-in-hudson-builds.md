---
ID: 696
post_title: >
  How to increase maven heapspace in
  hudson builds
author: matthew
post_date: 2009-11-23 15:22:07
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/how-to-increase-maven-heapspace-in-hudson-builds-696.html
published: true
syntaxhighlighter_encoded:
  - "1"
dsq_thread_id:
  - "160993514"
---
If your maven-built project fails in hudson (especially when you're using the assembly plugin) and it isn't a compile or test failure, check the console output. If it says "java.lang.OutOfMemoryError: Java heap space", you need to configure your hudson job to add heap space to the maven process.
<ol>
	<li>Navigate to your hudson job,</li>
	<li> click Configure,</li>
	<li> scroll down to the Build section, and</li>
	<li> click the Advanced button.</li>
	<li>Enter this into MAVEN_OPTS: <code>-Xmx512m -XX:MaxPermSize=128m</code></li>
</ol>

<!--more-->

Here's a complete stacktrace to help people find this tidbit:

<pre>java.lang.OutOfMemoryError: Java heap space
	at java.io.ByteArrayOutputStream.write(ByteArrayOutputStream.java:95)
	at sun.net.www.http.PosterOutputStream.write(PosterOutputStream.java:61)
	at org.apache.maven.wagon.AbstractWagon.transfer(AbstractWagon.java:492)
	at org.apache.maven.wagon.AbstractWagon.transfer(AbstractWagon.java:457)
	at org.apache.maven.wagon.AbstractWagon.putTransfer(AbstractWagon.java:411)
	at org.apache.maven.wagon.AbstractWagon.transfer(AbstractWagon.java:392)
	at org.apache.maven.wagon.AbstractWagon.putTransfer(AbstractWagon.java:365)
	at org.apache.maven.wagon.StreamWagon.put(StreamWagon.java:163)
	at org.apache.maven.artifact.manager.DefaultWagonManager.putRemoteFile(DefaultWagonManager.java:317)
	at org.apache.maven.artifact.manager.DefaultWagonManager.putArtifact(DefaultWagonManager.java:227)
	at org.apache.maven.artifact.deployer.DefaultArtifactDeployer.deploy(DefaultArtifactDeployer.java:107)
	at org.apache.maven.plugin.deploy.DeployMojo.execute(DeployMojo.java:190)
	at org.apache.maven.plugin.DefaultPluginManager.executeMojo(DefaultPluginManager.java:490)
	at hudson.maven.agent.PluginManagerInterceptor.executeMojo(PluginManagerInterceptor.java:182)
	at org.apache.maven.lifecycle.DefaultLifecycleExecutor.executeGoals(DefaultLifecycleExecutor.java:694)
	at org.apache.maven.lifecycle.DefaultLifecycleExecutor.executeGoalWithLifecycle(DefaultLifecycleExecutor.java:556)
	at org.apache.maven.lifecycle.DefaultLifecycleExecutor.executeGoal(DefaultLifecycleExecutor.java:535)
	at org.apache.maven.lifecycle.DefaultLifecycleExecutor.executeGoalAndHandleFailures(DefaultLifecycleExecutor.java:387)
	at org.apache.maven.lifecycle.DefaultLifecycleExecutor.executeTaskSegments(DefaultLifecycleExecutor.java:348)
	at org.apache.maven.lifecycle.DefaultLifecycleExecutor.execute(DefaultLifecycleExecutor.java:180)
	at org.apache.maven.lifecycle.LifecycleExecutorInterceptor.execute(LifecycleExecutorInterceptor.java:65)
	at org.apache.maven.DefaultMaven.doExecute(DefaultMaven.java:328)
	at org.apache.maven.DefaultMaven.execute(DefaultMaven.java:138)
	at org.apache.maven.cli.MavenCli.main(MavenCli.java:362)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:39)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:25)
	at java.lang.reflect.Method.invoke(Method.java:592)
	at org.codehaus.classworlds.Launcher.launchEnhanced(Launcher.java:315)
	at org.codehaus.classworlds.Launcher.launch(Launcher.java:255)
	at hudson.maven.agent.Main.launch(Main.java:165)
	at hudson.maven.MavenBuilder.call(MavenBuilder.java:159)</pre>