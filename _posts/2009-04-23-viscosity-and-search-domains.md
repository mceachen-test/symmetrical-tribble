---
ID: 379
post_title: Viscosity and Search Domains
author: matthew
post_date: 2009-04-23 21:48:32
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/viscosity-and-search-domains-379.html
published: true
dsq_thread_id:
  - "161127035"
---
I've really enjoyed <a href="http://www.viscosityvpn.com/">Viscosity</a> as my openvpn client for Mac OS X (instead of Tunnelblick). It's stable, bounces back after system sleep, and is well supported.

After upgrading from 1.0.3 to 1.0.4, I found that my search domain configuration from the Network Settings control panel wasn't being respected anymore while the VPN was up. I got the following reply (in 30 minutes, no less):

<blockquote>
The 1.0.3 behavior was actually a bug (your system should use any search domains associated with the VPN connection for security reasons, rather than local search domains).

You can specify search domains to use while connected instead like so:

1. Edit your connection in Viscosity
2. Click on the Advanced tab
3. Add the command "dhcp-option DOMAIN mydomain.com" (no quotes) where mydomain.com is your search domain
4. Repeat step 3 for each search domain you have
5. Click Save and try connecting
</blockquote>

These instructions worked perfectly. Thanks!