---
layout: post
title:  "Configuring OpenVPN on pfsense"
categories: Homelab Projects
---
I sometimes find it helpful to have access to my home network remotely. I chose to set up a VPN as I believe it offers the best combination of security and ease of configuration. For a homelab many people use a reverse proxy but I don't like the security compromises involved in this setup.

![](/assets/screenshots/vpn/2025-01-26_22-32_1.png)
### Wireguard vs OpenVPN
Both are open source. Wireguard is faster, more modern and has a cleaner codebase. OpenVPN is extremely widely supported and I have a little bit more experience using OpenVPN. I'd like to experiment with Wireguard at some point but for today I chose OpenVPN.

I'll be referencing the excellent [pfsense documentation](https://docs.netgate.com/pfsense/en/latest/recipes/openvpn-ra.html) which makes this easy. There's nothing worse than poor documentation.

### pfsense OpenVPN Wizard 

After starting the OpenVPN wizard the first option is to select what type of authentication you will be using. I'd like to try setting up a RADIUS server on my home network at some point but for the moment I'll be using simple Local User Access.
![](/assets/screenshots/vpn/2025-01-26_22-32_2.png)

Following along with the wizard, we need to ensure we have a certificate authority and appropriate certificates. 

![](/assets/screenshots/vpn/2025-01-26_22-33.png)

The major considerations for all of these options are the key type, length, algorithm and lifetime. The defaults here are totally fine for my use case, but you may want to consider a longer key or a shorter lifetime for better security.
![](/assets/screenshots/vpn/2025-01-27_01-56.png)

Next up I have to generate a server certificate, all the same considerations apply here. The maximum lifetime for server certificates is 398 days. When you select Server Certificate, the wizard will warn you if you have a longer lifetime selected.
![](/assets/screenshots/vpn/2025-01-26_22-40.png)
![](/assets/screenshots/vpn/2025-01-26_22-34.png)
![](/assets/screenshots/vpn/2025-01-26_22-35.png)

Now that the CA and Server Certificates are generated I can move on to the VPN configuration. Pfsense has selected sensible defaults here and the only thing I've changed is the Data Encryption Algorithm. I selected CHACHA20-POLY1305 and also selected it as the fallback algorithm. I don't want it falling back to a less secure method. I chose CHACHA20-POLY1305 because it is a modern algorithm with excellent performance that doesn't compromise on security.
![](/assets/screenshots/vpn/2025-01-26_22-40_1.png)
![](/assets/screenshots/vpn/2025-01-26_22-41.png)

My home network is a simple private class C network and I chose to use 192.168.169.0/24 for my VPN traffic. /24 is way more space than I really need, I'll never have more than 1 VPN connection at a time but I have plenty of space on my home network and this is a bit lazy. It's also easy to remember. I made a typo in this screenshot though, which I caught before moving on. Always double check your work before you hit next!
![](/assets/screenshots/vpn/2025-01-26_22-42.png)
I didn't need any of the advanced client settings, pfsense is already handling my home DNS and I was happy with that.
![](/assets/screenshots/vpn/2025-01-26_22-44.png)

The last consideration is whether I'd like the wizard to make a firewall rule for me or not. Considering my simple setup this will be a simple rule allowing access to UDP port 1195. I'm happy to let pfsense handle that for me, so I leave it checked.
![](/assets/screenshots/vpn/2025-01-26_22-44_1.png)

### Creating a user
At this point I needed a user that could actually connect to the VPN. I didn't have any users added in pfsense other than the default admin (With changed credentials for security, of course!). Under System -> User Manager, it's easy to add a user. Nothing had to be changed here other than a username and password, as well as checking the box to create a certificate. Selecting the CA for the VPN I had just created earlier.
![](/assets/screenshots/vpn/2025-01-26_22-37.png)
![](/assets/screenshots/vpn/2025-01-26_22-38.png)

### Client Export
Now that the VPN server is set up and users are also configured, back to the VPN page to use the client export. This page is awesome, it has a ton of features allowing exporting of OpenVPN packages for a wide variety of personal and enterprise use cases. For my personal use I don't need any of these options, and I just scrolled to the bottom and exported the install package. I'm testing this on a windows laptop and the install package for windows is a simple few clicks, installing everything including the user credentials. After the install it was immediately ready to use. 
![](/assets/screenshots/vpn/2025-01-26_22-46.png)
![](/assets/screenshots/vpn/2025-01-26_22-46_1.png)

### Testing
First attempt testing the VPN after setup, it works! This was a fun and easy setup that paid off. 
![](/assets/screenshots/vpn/2025-01-27_02-58.png)