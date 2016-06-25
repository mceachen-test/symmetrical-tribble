---
ID: 1123
post_title: >
  Sandbox telegrams, or, how your Chrome
  extension can interact with page content
  scripts
author: matthew
post_date: 2011-03-24 23:22:41
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/sandbox-telegrams-or-how-your-chrome-extension-can-interact-with-page-content-scripts-1123.html
published: true
dsq_thread_id:
  - "262640965"
---
In <a href="http://adgrok.com">AdGrok's GrokBar</a>, we inject a "heads-up display" on pages that the user is advertising. The heads-up display is actually an <code>iframe</code> that's positioned within a browser-extension-injected <code>div</code>, and that iframe renders content from our secure server farm. You can see a demo video <a href="http://adgrok.com/features#see">here</a> (and see that we truly spared no expense on the voice-over talent!).

I wanted to make our extension's button-click incant a javascript method that was inside the GrokBar's iframe. I found this <a href="http://code.google.com/chrome/extensions/content_scripts.html#host-page-communication">horrible hack</a>, but every time a javascript timer scrapes a hidden DOM element, or mucks with the URL fragment of an iframe src in order to send messages, the code gods kick a puppy. 

<!--more-->

<h3>Just executeScript?</h3>

So I first tried to use <a href="http://code.google.com/chrome/extensions/tabs.html#method-executeScript">executeScript</a>, but by default, executeScript only runs against the tab's topmost main frame. By using the <code>allFrames</code> option, you can get your executeScript to run inside the iframe, too.

I knew about Chrome's <a href="http://code.google.com/chrome/extensions/content_scripts.html#videos">isolated worlds</a>, but I didn't realize that extensions are considered a different world from page content scripts -- my executeScript failed from "unknown method."

<h3>HTML5 postMessage?</h3>

I then thought to use <a href="https://developer.mozilla.org/en/DOM/window.postMessage">postMessage</a> from within the extension, but Chrome's sandboxing prevents access to an iframe's <code>window</code> object, which is what you'd incant the postMessage against, from anywhere except in the scope of the iframe. Whether this is <a href="http://stackoverflow.com/questions/4073057/why-cant-my-chrome-extension-use-html5-postmessage-to-communicate-with-a-frame-i">a bug or a design decision</a>, I don't really care -- the jist is that it doesn't work.

<h3>You got your postMessage in my executeScript! <a href="http://www.youtube.com/watch?v=DJLDF6qZUX0/">Delicious!</a></h3>

But... what if you combined them? 

Call postMessage with a data payload, against itself, using executeScript. A previously established message listener within the iframe, and within the context of the javascript sandbox, could then receive the postMessage and dispatch whatever method was necessary. No nasty DOM scraping! woot! The puppies are saved!

So, within the chrome extension, we call:

<pre lang="javascript">
chrome.tabs.executeScript(tabId, {
  code: "window.postMessage(\"hello\",\"" + ADGROK.server()+ "\")",
  allFrames: true
});</pre>

Assume that ADGROK.server() returns something like <code>https://secure.adgrok.com</code>, so we know the postMessage will only run on the iframe we want it to.

Within the iframe, we have this javascript code:

<pre lang="javascript">
  window.addEventListener("message", on_message, false);
  function on_message(e) {
    if (e.origin === window.location.origin) { // only accept local messages
      if (e.data == "hello") {
        // call some other locally-scoped javascript method here...
      }
    }
  }
</pre>