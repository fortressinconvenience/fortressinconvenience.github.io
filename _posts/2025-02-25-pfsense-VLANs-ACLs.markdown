---
layout: post
title:  "pfSense, VLANs, ACLs and NanoKVM"
categories: Homelab Projects
---
I have a silly problem, when I need to access my headless TrueNAS server I don't have a monitor nearby. It's mounted in my rack and it's stable enough that I only need to access the bios every few years on average. Still it's pretty annoying to go grab my spare monitor out of storage and bring it over. This is a really small problem and it's not a problem worth spending much time or money solving.

It's been pretty interesting though seeing the rise of cheap KVM devices and some really interesting projects. [PiKVM](https://pikvm.org/), [JetKVM](https://jetkvm.com/) and [NanoKVM](https://github.com/sipeed/NanoKVM) . Out of these options I think JetKVM really hits the sweet spot for price, security and performance. That being said the JetKVM works out to around $120 canadian after shipping and the scope of my problem isn't fundamentally worth $120. On the other hand a NanoKVM device can be had for $35 Canadian shipped straight from China.

Plugging a device from AliExpress straight into your network is a horrendously bad idea and that's especially true of something like a KVM that can remotely access the device it's plugged into. It's nice to see NanoKVM has made their code open source this month and I believe they are making an effort to be transparent. I also believe now that the code is open source that many generous folks will contribute and fix the issues with the codebase. Despite that I still don't believe it's currently it a state I can truly trust on my network.

While I am trying to solve an insignificant issue(carrying a monitor up the stairs), I think this situation is representative of a common enterprise problem. Many companies rely on hardware that is far past it's end of life but there's no replacement. These devices have security flaws, sometimes seriously well known major security issues but the company still relies on them. I'm considering this a small exercise in placing an unsafe device on my network as securely as possible.

First and foremost, I don't need to use the NanoKVM often. I will take the simple security precaution of unplugging it when it's not in use. Beyond that I need to quarantine it on my network. I will need to prevent it from communicating over both layer 2 and 3 except to my one admin machine. Problematically I have an unmanaged switch behind my pfSense box and I won't be able to segregate layer 2 traffic. A full router-on-a-stick style setup is possible with pfSense and a managed switch but I'm going to accept the limitations of the equipment I have(Any maybe hunt ebay for a used managed switch.).

First I'll start by making a VLAN under Interfaces > Assignments > VLANs. 

![](/assets/screenshots/pfsenseVLAN/2025-02-25_02-25.png)

This is my home network and I really don't use many vlans, I'm going with a fun and memorable vlan number 1111. I select the LAN interface. The VLAN priority tag here is for QoS and more specifically P802.1p. I don't really need to set this on my home network but I'll set it anyway. The settings suggested by the standard are 4 for video (<100ms) and 5 for Voice (<10ms). A KVM will be transmitting video but I do want the lowest latency to make interaction smooth. I've chosen 5 for this reason.

![](/assets/screenshots/pfsenseVLAN/2025-02-25_04-19.png)

Next I configure the VLAN. I've opted for a static IP and given it the 192.168.170.1 IP with a /29 subnet. This will give me 6 host addresses, with one taken by the VLAN itself. I considered /30, that would give me two host addresses with one taken by the VLAN and leave one for the NanoKVM, but I might want another device in this VLAN in the future. 

![](/assets/screenshots/pfsenseVLAN/2025-02-25_02-48.png)

Next I double check the DHCP settings for the now configured VLAN. I don't want DHCP on this VLAN and I'm just confirming it's turned off and no pools are assigned. I will statically assign any devices I want in this VLAN. 

![](/assets/screenshots/pfsenseVLAN/2025-02-25_02-51.png)

Finally I configure the firewall rules for the VLAN. Firewall rules in pfSense follow the standard of being applied in order. The other thing to know is that inter-VLAN communication is handled entirely by firewall rules. This makes configuration pretty simple.

![](/assets/screenshots/pfsenseVLAN/2025-02-25_03-00.png)

The first rule I create here is a "deny any any" rule. Simply denying all traffic from all protocols. Though this is the first rule I create, it will have to be the last rule in the list. I will use the add button with an up arrow to create the next rules above this one.

![](/assets/screenshots/pfsenseVLAN/2025-02-25_03-04.png)

Next I need to make two rules, NanoKVM is accessed via a web interface over the common HTTP and HTTPS ports. I choose TCP as the protocol and choose "address" for both the source and destination. For the source I enter the IP of my admin machine and the destination I enter the static IP I have assigned on the VLAN for NanoKVM. For the destination I choose HTTP and HTTPS respectively for each rule.
![](/assets/screenshots/pfsenseVLAN/2025-02-25_03-52.png)
![](/assets/screenshots/pfsenseVLAN/2025-02-25_03-52_1.png)
Finally I review the rules I created and ensure everything I input was correct.
![](/assets/screenshots/pfsenseVLAN/2025-02-25_03-52_2.png)

That's it! pfSense provides a great interface that makes this simple and easy. In the future I definitely want to upgrade to a managed switch with a proper router-on-a-stick setup but for the moment I'm pretty happy with this.