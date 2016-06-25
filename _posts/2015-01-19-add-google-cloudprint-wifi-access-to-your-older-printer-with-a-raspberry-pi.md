---
ID: 1342
post_title: 'Add Google cloudprint &#038; wifi access to your older printer with a headless $35 Raspberry Pi'
author: matthew
post_date: 2015-01-19 13:22:54
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/add-google-cloudprint-wifi-access-to-your-older-printer-with-a-raspberry-pi-1342.html
published: true
dsq_thread_id:
  - "3436049079"
---
<blockquote>Update 2015/08/01: The broken cloudprint package that these instructions used to reference have been replaced with Google's <a href="https://github.com/google/cups-connector">new connector</a>.
</blockquote>

There are <a href="http://www.howtogeek.com/169566/how-to-turn-a-raspberry-pi-into-a-google-cloud-print-server/">several</a> <a href="https://support.google.com/a/answer/2906017?hl=en">articles</a> that proclaim how easy it is to set up a Chromium on a $35 Raspberry Pi to let you print to your printer with your Chromebook or mobile devices.

<strong>One unfortunate problem; none of those instructions work anymore.</strong>

<!--more-->

Google has dropped Cloud Print support for the Chromium version compiled for Rasbian, v22. <a href="http://raspberrypi.stackexchange.com/questions/19365/how-can-i-install-the-latest-version-of-chromium">Using Arch Linux and recompiling Chromium</a> might be a solution if you don't mind keeping a monitor attached to your Pi, but that's a nonstarter for me; I need something that just works. There's a <a href="https://github.com/armooo/cloudprint">python cloudprint script</a> that used to work (that these instructions used to recommend using!), but it <a href="https://github.com/davesteele/cloudprint-service/issues/25">no longer supports LTS</a>, so that's not an option either.

Luckily, a Google has open-sourced <a href="https://github.com/google/cups-connector">a connector</a> that exposes printers under the <a href="https://www.cups.org/">Common Unix Printing Service, CUPS</a>. We'll set up the Raspberry Pi to be headless, install and set up CUPS, install Go, and finally build and install the cloudprint connector.

<h4>Preparation: Console boot your Pi</h4>

Use <code><a href="http://www.raspberrypi.org/documentation/configuration/raspi-config.md">raspi-config</a></code> to 
<ol>
	<li>change the pi password from the default and</li> 
	<li>set boot to terminal</li>
</ol>

<h4>Preparation: Set up WiFi on your Pi</h4>

If you're using a Wifi dongle, don't bother with the GUI setup. Assuming you're using a recent WPA-enabled router, just edit <code>/etc/networking/interfaces</code>:

<pre class="show-lang:2 mark:6-12 decode:true highlight:0" >auto lo

iface lo inet loopback
iface eth0 inet dhcp

allow-hotplug wlan0
auto wlan0

iface wlan0 inet dhcp
        wpa-ssid "ssid"
        wpa-psk "password"
        wireless-power off
</pre> 

Make sure you replace <strong>ssid</strong> and <strong>password</strong> appropriately, and reboot to make sure everything works. You should see the network DHCP request message during the next boot.

The <code>wireless-power off</code> <a href="http://www.raspberrypi.org/forums/viewtopic.php?t=46569&p=666920">disables power management</a>. By default the wifi radio will turn off when idle (which it will be most of the time), but that means it can't receive new print jobs from the cloud or local systems (via CUPS).

If you need to support multiple routers (so it works in multiple houses, for example), then <tt>/etc/network/interfaces</tt> should look like:

<pre>
auto wlan0
allow-hotplug wlan0
iface wlan0 inet manual
wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf
wireless-power off
</pre>

and <tt>/etc/wpa_supplicant/wpa_supplicant.conf</tt> should look something like the following. Replace SSID1* and SSID2*, <tt>proto</tt>, and <tt>pairwise</tt> accordingly:

<pre>
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1

network={
    ssid="SSID1"
    psk="SSID1_PASSWORD"
    proto=RSN
    key_mgmt=WPA-PSK
    pairwise=CCMP
    auth_alg=OPEN
}

network={
    ssid="SSID2"
    psk="SSID2_PASSWORD"
    proto=WPA
    key_mgmt=WPA-PSK
    pairwise=TKIP
    auth_alg=OPEN
}
</pre>

<h4>Set up automatic security updates</h4>

<a href="http://raspberrypi.stackexchange.com/a/4706">Follow these steps</a> to configure the <tt>unattended-upgrades</tt> package:

<pre class="lang:bash">
sudo apt-get install unattended-upgrades
</pre>

and then open <tt>/etc/apt/apt.conf.d/50unattended-upgrades</tt> and make sure it's set up how you'd like it.

<h4>Install CUPS</h4>

3. Install CUPS:

<pre class="lang:bash">
sudo apt-get install cups cups-bsd
</pre>

<h4>Set up the printer(s) in CUPS</h4>

1. Let the <tt>pi</tt> user manage printers:

<pre class="lang:bash">
sudo cupsctl --remote-admin
sudo adduser pi lpadmin
</pre>

2. Assuming your desktop is unix-like, you can remotely administer CUPS via an ssh tunnel. Change <tt>192.168.0.100</tt> to the address of your Pi:

<pre class="lang:bash">
ssh -L 1631:localhost:631 pi@192.168.0.100
</pre>

3. Add the printer(s) to CUPS

Point a browser at <a href="http://localhost:1631">http://localhost:1631</a>. Click "Admin", then click "Add Printer." Use user "pi" and your password.

If you don't see the Model in the setup section, the PPD should be available from your printer's manufacturer's support website. 

If you get stuck, <a href="http://www.cups.org/doc-1.1/sum.html">RTFM</a>, it's pretty good.

Also, make sure you make a test print with your newly set up printer to make sure everything is working so far.

<h4>Install GCP</h4>

Go to the <a href="https://github.com/google/cups-connector/wiki/Install">Google Cloud Print connector installation wiki</a>, and follow the instructions.

Reboot your Pi and make sure the connector is running with <tt>gcp-cups-connector-util -monitor</tt>.

Printing should now work! 

<h3>Extra Credit</h3>

<h4>Safe shut downs</h4>
If you're tired of having your sdcard get corrupted when you unplug instead of shutting down gracefully, <a href="https://matthew.mceachen.us/blog/give-your-raspberry-pi-a-shutdown-switch-for-free-1397.html">follow these steps</a>.

<h4>Remote administration with ssh</h4>
If the Pi will be living behind a NAT and not physically accessible, you may want to <a href="/blog/durably-access-remote-machines-with-autossh-1424.html">set up a durable ssh tunnel</a> from the Pi back to a machine you have access to.

<b>Update 20150310:</b> Switched to accessing cups via an ssh port forward, and fixed the cloudprint command. Thanks, Niklas Klingspetz!

<b>Update 20150801:</b> Gave up on the python cloudprint connector, and switched to Google's connector.

<b>Update 20150914:</b> Google's connector has been updated to not require upgrading to Jessie. <a href="https://github.com/google/cups-connector/wiki/Raspberry-Pi-step-by-step">Instructions are here.</a>

<b>Update 20150930:</b> OK, GCP installation instructions changed again (srsly), so I'm going to just point to those instructions from here on out.