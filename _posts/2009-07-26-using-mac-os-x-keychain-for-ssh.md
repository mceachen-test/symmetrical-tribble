---
ID: 526
post_title: 'Using Mac OS X 10.5&#8217;s keychain for ssh'
author: matthew
post_date: 2009-07-26 16:09:19
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/using-mac-os-x-keychain-for-ssh-526.html
published: true
syntaxhighlighter_encoded:
  - "1"
dsq_thread_id:
  - "160993466"
---
The version of ssh that comes with Mac OS X 10.5.6 has a <code>-K</code> option that stores your passphrases in your system's <a href="file://Applications/Utilities/Keychain Access.app">keychain</a>.

Run this:
<pre>ssh-add -K [path to private keyfile]</pre>
Provide your passphrase once when asked, and keychain will provide the passphrase for you automatically. You should probably enable "Require password to wake this computer from sleep or screen saver" in the Security pane of the System Preferences if you decide to do this.

If you see
<pre>$ ssh-add -K
<strong>ssh-add: illegal option -- K</strong>
</pre>
it's because you're using the macports (or fink) version of ssh. (run 'which ssh' to find out). With macports, uninstall the "openssh" package:

<pre>sudo port uninstall openssh</pre>

The '-K' option was discovered courtesy of <a href="http://www-uxsup.csx.cam.ac.uk/~aia21/osx/leopard-ssh.html">http://www-uxsup.csx.cam.ac.uk/~aia21/osx/leopard-ssh.html</a>.

<a href="http://kimmo.suominen.com/docs/ssh/">http://kimmo.suominen.com/docs/ssh/</a> has some excellent ssh documentation.