---
ID: 43
post_title: Ant bash completion on Mac OS X
author: matthew
post_date: 2008-04-24 15:08:12
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/ant-bash-completion-on-mac-os-x-43.html
published: true
btc_comment_counts:
  - 'a:0:{}'
btc_comment_summary:
  - 'a:0:{}'
dsq_thread_id:
  - "160993274"
---
<a href="http://www.gnu.org/software/bash/">bash</a> will do tab-completion for <a href="http://ant.apache.org/">ant</a> targets on Debian/Ubuntu boxes out-of-the-box. If you haven't upgraded lately, you may need to:
<pre>
sudo apt-get install bash-completion
</pre>

On Mac OS X, it needs a bit of massaging. First install the macports version of bash-completion and ant:
<pre>
sudo port install bash-completion apache-ant
</pre>

Then add this to the end of your <tt>~/.bashrc</tt>:
<pre>
if [ -f /opt/local/etc/bash_completion ]; then
. /opt/local/etc/bash_completion
fi
complete -C /opt/local/share/java/apache-ant/bin/complete-ant-cmd.pl ant
</pre>

See <a href="http://marius.scurtescu.com/2005/03/23/ant_bash_completion">http://marius.scurtescu.com/2005/03/23/ant_bash_completion</a> for Windows instructions.