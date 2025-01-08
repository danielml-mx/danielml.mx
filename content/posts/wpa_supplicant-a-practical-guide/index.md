+++
date = '2024-12-13T17:13:36-06:00'
draft = false
title = 'WPA Supplicant - A Practical Guide'
image = "cover.png"
tags = ["linux 🐧"]
+++

<!DOCTYPE html>
<html lang="en">
<head>
<title>WPA Supplicant - A practical guide</title>
<link rel='stylesheet' type='text/css' href='../style.css'>
<meta charset='utf-8'/>
</head>
<body>
<p style="text-align: center"><i>Esta es una de mis publicaciones viejas, escrita alrededor de 2022.</i></p>

<main>

<h2>Index</h2>
<ol>
<li>
<a href="#Preface">
<span class="toctext">Preface</span>
</a>
</li>
<li>
<a href="#Installation">
<span class="toctext">Installation</span>
</a>
</li>
<li>
<a href="#Using the correct configuration file">
<span class="toctext">Using the correct configuration file</span>
</a>
<li>
<a href="#Editing the configuration file">
<span class="toctext">Editing the configuration file</span>
</a>
</li>
<ul>
<li>
<a href="#Connecting to a network you know the credentials to">
<span class="toctext">Connecting to a network you know the credentials to</span>
</a>
</li>
<li>
<a href="#Connecting to an enterprise network">
<span class="toctext">Connecting to an enterprise network</span>
</a>
</li>
<li>
<a href="#Scanning for and connecting to open networks">
<span class="toctext">Scanning for and connecting to open networks</span>
</a>
</li>
<li>
<a href="#Managing multiple networks">
<span class="toctext">Managing multiple networks</span>
</a>
</li>
<li>
<a href="#Protecting your passphrases">
<span class="toctext">Protecting your passphrases</span>
</a>
</li>
</ul>
<li>
<a href="#Setting up the service">
<span class="toctext">Setting up the service</span>
</a>
</li>
<ul>
<li>
<a href="#SystemD">
<span class="toctext">SystemD</span>
</a>
</li>
<li>
<a href="#Runit">
<span class="toctext">Runit</span>
</a>
</li>
<li>
<a href="#OpenRC">
<span class="toctext">OpenRC</span>
</a>
</li>
</ul>
<li>
<a href="#See also">
<span class="toctext">See also</span>
</a>
</li>
</ol>

<h2>
<span id="Preface">Preface</span>
</h2>
<p>
This guide is meant to be a quick setup guide for <i>wpa_supplicant</i>, designed primarily with Arch (<i>SystemD</i>) and Artix (<i>runit</i>) in mind. If you have no idea how to set up <i>wpa_supplicant</i>, you should follow it thoroughly. If you have a very specific doubt, just skip to the section which will hopefully help you solve your problems; the sections are written independently from each other unless stated otherwise.
</p>

<h2>
<span id="Installation">Installation</span>
</h2>
<h5>Note: Of course, installing these programs will require an active internet connection, which you probably do not have at this point. The installation should be done in the live ISO in which you also installed your operating system.</h5>
<p>
<i>wpa_supplicant</i> is sure to be found in your distribution's official repositories. You should also install <i>dhcpcd</i> to get an IP address and complete the connection process. For default Arch-based, <i>SystemD</i> distributions, a simple
</p>
<code># pacman -S wpa_supplicant dhcpcd</code> 
<p>should do.</p>
<!-- <style type="text/css"> code { background-color: gray; color: blue; } </style> -->
<p>
However, in distributions like Artix and Parabola, specific service scripts need to be installed for your init system. For example, using <i>runit</i> as an init system for Artix, the following packages would also need to be installed:
</p>
<code># pacman -S wpa_supplicant-runit dhcpcd-runit</code>
<!--
<p>
And for <i>OpenRC</i>:
</p>
<code># pacman -S wpa_supplicant-openrc dhcpcd-openrc</code>
-->

<h2>
<span id="Using the correct configuration file">Using the correct configuration file</span>
</h2>
<h5>Note: WPA Supplicant is flexible enough to use any file as a configuration file, so none of these steps are technically necessary, but they are convention and will spare you a lot of confusion and file editting.</h5>
<p>
Before connecting to the internet, you should know through which wireless interface you can do this with. Most often this will be <code>wlp2s0</code> or <code>wlan0</code>, but you can quickly verify this by doing:
</p>
<code>$ ip a</code>
<p>Entries starting with "wlp" or "wlan" are the interfaces capable of wireless connection. You may have multiple of these if you have multiple wireless adapters, in which case you should choose one.<p>
<p>Configuration files are conventionally placed under <code>/etc/wpa_supplicant/</code>, but their name changes according to the interface you wish to use. They follow the format <code>wpa_supplicant-<i>[interface]</i>.conf</code>, so if your interface is <code>wlan0</code>, your complete file path would be <code>/etc/wpa_supplicant/wpa_supplicant-wlan0.conf</code>.</p>

<h2>
<span id="Editing the configuration file">Editing the configuration file</span>
</h2>
<h3>
<span id="Connecting to a network you know the credentials to">Connecting to a network you know the credentials to</span>
</h3>
<p>
Adding a standard WPA2 network is most commonly done through <i>wpa_passphrase</i>. This program takes in two simple arguments: the network's SSID (name) and its passphrase. Take a look at this intuitive example:
</p>
<pre style="margin-bottom: 0; border-bottom:none; padding-bottom:0.8em;">$ wpa_passphrase MyNetwork password123</pre>
<pre style="margin-top: 0; border-top-style:dashed; padding-top: 0.8em;">
network={
ssid="MyNetwork"
#psk="password123"
psk=1f1503df6a9182b5936a027973a426df3954bb0ff5d4add55753fe37b02a55f1
}
</pre>
<p>This output can then be redirected into your configuration file in the following way:</p>
<pre># wpa_passphrase MyNetwork password123 >> /etc/wpa_supplicant/wpa_supplicant-<i>[interface]</i>.conf</pre>
<p>or if you prefer to use <i>sudo</i> over a root shell:</p>
<pre>$ wpa_passphrase MyNetwork password123 | sudo tee -a /etc/wpa_supplicant/wpa_supplicant-<i>[interface]</i>.conf</pre>
<p>
<a href="#Protecting your passphrases">
<span class="toctext">Protecting your passphrases</span>
</a>
presents more options for those concerned with security, but as a bare minimum manually deleting the plaintext versions of your passphrases should do.
</p>
<p>At this point restarting the service will load in the edited configuration file and, if the credentials are valid, will connect you to the internet. If you do not know how to do this, read <a href="#Setting up the service"><span class="toctext">Setting up the service</span></a>.</p>
<h3>
<span id="Connecting to an enterprise network">Connecting to an enterprise network</span>
</h3>
<p>
Enterprise networks do not have a program like <i>wpa_passphrase</i> to fill in entries, but they follow a similar format. Still, if you need to connect to a network of this kind, you will have to manually add the entry. These networks utilize the following format:
</p>
<pre style="margin-bottom: 0; border-bottom:none; padding-bottom:0.8em;">/etc/wpa_supplicant/wpa_supplicant-<i>[interface]</i>.conf</pre>
<pre style="margin-top: 0; border-top-style:dashed; padding-top: 0.8em;">
network={
ssid="AnEnterpriseNetwork"
key_mgmt=WPA-EAP
eap=PEAP
identity="user_name"
password="user_password"
phase2="autheap=MSCHAPV2"
}
</pre>
<p>
Considering that you are utilizing a plain-text password and are unable to get around it, it is advisable that you follow the strictest protocols dictated in <a href="#Protecting your passphrases"><span class="toctext">Protecting your passphrases</span></a>.
</p>
<h3>
<span id="Scanning for and connecting to open networks">Scanning for and connecting to open networks</span>
</h3>
<p>To enable network scanning, the following line must be added in the configuration file, preferably at the beginning: </p>
<pre style="margin-bottom: 0; border-bottom:none; padding-bottom:0.8em;">/etc/wpa_supplicant/wpa_supplicant-<i>[interface]</i>.conf</pre>
<pre style="margin-top: 0; border-top-style:dashed; padding-top: 0.8em;">ctrl_interface=/run/wpa_supplicant</pre>
<p>
To actually scan for networks, a separate program must be used, in a similar vein to <i>wpa_passphrase</i>. This program is <i>wpa_cli</i>, and does require root privileges. The program will start interactively with a simple:
</p>
<pre># wpa_cli</pre>
<p>
You should be greeted by some text and a simple <code>></code>, indicating you are in interactive mode. To begin a scan, all that is necessary is to execute the command:
</p>
<pre>> scan</pre>
<p>
which, after some time, should yield the output:
</p>
<pre>
> scan
OK
<3>CTRL-EVENT-SCAN-STARTED
<3>CTRL-EVENT-SCAN-RESULTS
</pre>
<p>
point at which you can see the scan's results by doing:
</p>
<pre>> scan_results</pre>
<p>
Networks detected withing your range should be presented, and from them, open networks should look something like the following (notice the lack of flags when compared to a normal entry):
</p>
<pre>
> scan_results
bssid / frequency / signal level / flags / ssid
XX:XX:XX:XX:XX:XX   XXXX-XX     [WPA2-PSK-CCMP][WPS][ESS]       AClosedAndProtectedNetwork
XX:XX:XX:XX:XX:XX   XXXX-XX     [ESS]       AnOpenAndInsecureNetwork
</pre>
<p>
You should know that there is a way to interactively connect to networks and save their credentials (or lack thereof) to the configuration file automatically, but frankly since I only ever use <i>wpa_cli</i> to connect to open networks I choose to just manually add the short entry for this kind of network, like so:
</p>
<pre style="margin-bottom: 0; border-bottom:none; padding-bottom:0.8em;">/etc/wpa_supplicant/wpa_supplicant-<i>[interface]</i>.conf</pre>
<pre style="margin-top: 0; border-top-style:dashed; padding-top: 0.8em;">
network={
   ssid="AnOpenAndInsecureNetwork"
   key_mgmt=NONE
}
</pre>
<h3>
<span id="Managing multiple networks">Managing multiple networks</span>
</h3>
<p>
Sometimes multiple networks are available in the same area, in which case you might prefer using one over the other. This can be easily done through the use of prioritization. In <i>wpa_supplicant</i>'s configuration file, this is done with the <code>priority</code> parameter inside the network entries. The networks with the highest priority will actually be those with the highest value, so the highest priority a network can have is 999.
</p>
<p>
To illustrate its usage, all previous examples are gathered here, organized with the home network always having the highest priority, the enterprise network having the second highest and the open network having the lowest priority possible.
</p>
<pre style="margin-bottom: 0; border-bottom:none; padding-bottom:0.8em;">/etc/wpa_supplicant/wpa_supplicant-<i>[interface]</i>.conf</pre>
<pre style="margin-top: 0; border-top-style:dashed; padding-top: 0.8em;">
ctrl_interface=/run/wpa_supplicant

\# Home network
network={
ssid="MyNetwork"
psk=1f1503df6a9182b5936a027973a426df3954bb0ff5d4add55753fe37b02a55f1
priority=999
}

\# Enterprise network
network={
ssid="AnEnterpriseNetwork"
key_mgmt=WPA-EAP
eap=PEAP
identity="user_name"
password="user_password"
phase2="autheap=MSCHAPV2"
priority=998
}

\# Open network
network={
   ssid="AnOpenAndInsecureNetwork"
   key_mgmt=NONE
   priority=-999
}
</pre>

<h3>
<span id="Protecting your passphrases">Protecting your passphrases</span>
</h3>
<p>
Your configuration file will hold the credentials to multiple networks, and considering how much sensitive information can go through these, it is important you protect them.
</p>
The most basic form of protection would be to delete plaintext password from your configuration file, since they are already made redundant by the ciphered passphrases. This step already goes a long way in protecting your credentials, and if you do not share your computer with other people, is as far as you would have to go. Anything else is just for reinforcement's sake.
<p>
If you wish to keep your plaintext passphrases and still want to prevent other users from seeing them, you can change your configuration file's permissions so that only privileged users can see them. Assuming that the file and directory already belong to root (which they will if you haven not explicitly changed it), all you need to do is to change permissions on the file so that other users do not have read or execute permissions. 
</p>
<pre># chmod 600 /etc/wpa_supplicant/wpa_supplicant-<i>[interface]</i>.conf</pre>
<p>
Finally, if you insist on protecting your passphrases, you should also consider clearing your shell's history file so that it is not readable there.
</p>

<h2>
<span id="Setting up the service">Setting up the service</span>
</h2>
<p>
Starting the <i>wpa_supplicant</i> service is highly dependent on the init system you are running. This particular guide covers enabling the services under <i>SystemD</i> and <i>runit</i>, but enabling it under other init systems should not be too dissimilar.
</p>
<h3>
<span id="SystemD">SystemD</span>
</h3>
<p>If you have organized your configuration file as detailed in the previous section, enabling the service should be as simple as:</p>
<code># systemctl enable dhcpcd wpa_supplicant@<i>[interface]</i> --now</code>
<h3>
<span id="Runit">Runit</span>
</h3>
<p>
Unlike with <i>SystemD</i>, you do not want to just initialize the service. You have to first explicitly configure the script to point to the configuration file and interface you wish to use. This script is located in <code>/etc/runit/sv/wpa_supplicant/run</code>. The "official" way of doing this is by creating a <code>conf</code> file in the same directory and populating with the variables <code>run</code> calls for, like this:
</p>
<pre style="margin-bottom: 0; border-bottom:none; padding-bottom:0.8em;">/etc/runit/sv/wpa_supplicant/conf</pre>
<pre style="margin-top: 0; border-top-style:dashed; padding-top: 0.8em;">
#!/bin/sh
CONF_FILE=/etc/wpa_supplicant/wpa_supplicant-wlp2s0.conf
WPA_INTERFACE=wlp2s0
</pre>
<p>Honestly, I find this a bit too redundant, which is why I prefer to edit the <i>run</i> file directly, like so:</p>
<pre style="margin-bottom: 0; border-bottom:none; padding-bottom:0.8em;">/etc/runit/sv/wpa_supplicant/run</pre>
<pre style="margin-top: 0; border-top-style:dashed; padding-top: 0.8em;">
#!/bin/sh
#[ -r ./conf ] && . ./conf
exec 2>&1
#exec wpa_supplicant -c ${CONF_FILE:=/etc/wpa_supplicant/wpa_supplicant.conf} -i ${WPA_INTERFACE:=wlan0} ${OPTS:=-s}
exec wpa_supplicant -c /etc/wpa_supplicant/wpa_supplicant-wlp2s0.conf -i wlp2s0 ${OPTS:=-s}
</pre>
<p>
Either way will work just fine. Now you can proceed to actually enabling the services, which in <i>runit</i> is done by doing:
</p>
<code># ln -s /etc/runit/sv/dhcpcd /run/runit/dhcpcd</code>
<br/>
<code># ln -s /etc/runit/sv/wpa_supplicant /run/runit/wpa_supplicant</code>
<p>
Finally, to start the services:
</p>
<code># sv start dhcpcd wpa_supplicant</code>

<h3>
<span id="OpenRC">OpenRC</span>
</h3>
<p>
Edit /etc/init.d/wpa_supplicant to include the desired interface and configuration file.
</p>
<pre style="margin-bottom: 0; border-bottom:none; padding-bottom:0.8em;">/etc/init.d/wpa_supplicant</pre>
<pre style="margin-top: 0; border-top-style:dashed; padding-top: 0.8em;">
#!/usr/bin/openrc-run
# Copyright (c) 2009 Roy Marples <roy@marples.name>
# All rights reserved. Released under the 2-clause BSD license.

command=/usr/bin/wpa_supplicant
\: ${wpa_supplicant_conf:=/etc/wpa_supplicant/wpa_supplicant.conf}
wpa_supplicant_if=${wpa_supplicant_if:+-i}$wpa_supplicant_if
#command_args="$wpa_supplicant_args -B -c$wpa_supplicant_conf $wpa_supplicant_if"
command_args="$wpa_supplicant_args -B -c/etc/wpa_supplicant/wpa_supplicant-wlan0.conf wlan0"
name="WPA Supplicant Daemon"

[...]
</pre>
<p>
Now just enable the service:
<p>
<pre class="code-commands">
# rc-update add dhcpcd default
# rc-update add wpa_supplicant default</pre>

<h2>
<span id="See also">See also</span>
</h2>
<ul>
<li>
<a href="https://wiki.archlinux.org/title/Wpa_supplicant">
<span class="toctext"><i>wpa_supplicant</i> on the Arch Wiki: </span>
</a>
Excellent resource if you want to go more in-depth and plan to do something more complicated.
</li>
<li>
<a href="https://linux.die.net/man/8/wpa_supplicant">
<span class="toctext">wpa_supplicant(8):</span>
</a>
Manual page for <i>wpa_supplicant</i>.
</li>
<li>
<a href="https://linux.die.net/man/5/wpa_supplicant.conf">
<span class="toctext">wpa_supplicant.conf(5):</span>
</a>
Manual page for <i>wpa_supplicant</i>'s configuration file.
</li>
<li>
<a href="https://linux.die.net/man/8/wpa_passphrase">
<span class="toctext">wpa_passphrase(8):</span>
</a>
Manual page for <i>wpa_passphrase</i>.
</li>
</ul>

</main>
</html>
