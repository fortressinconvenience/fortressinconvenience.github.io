---
layout: post
title:  "TrueNAS Core to Scale Migration"
categories: Homelab Projects
---
I've been running TrueNAS for almost a decade now on my home NAS. I only have good things to say about it, it's easy to use, stable and free for personal use.

TrueNAS in the past has always been FreeBSD(Now TrueNAS Core) based but a few years ago iXsystems began a move to the Debian based TrueNAS Scale. My home system has been pretty stable, I didn't feel like risking a move to Scale while it was still in Beta. Time has trucked on and Core has entered a soft EOL while Scale has matured.

I don't have strong opinions on Linux vs FreeBSD, I think they're both great. There's been controversy over this change but I only really care about having a nice stable NAS system. I chose to upgrade to Scale to ensure I continued getting updates and service for my system, but I am a bit more comfortable in Linux so this was an advantage for me too.

### Installation

I started with the WebUI upgrade tool, it gave me significant warnings and suggested a manual upgrade. I wasn't too worried about data loss, I knew I could install clean and easily import my data pools without issues. Given that worst case scenario, I went ahead and tried it. After 20 minutes of installing, the system reboots and failed to boot. What did I expect?

I grabbed a USB drive and moved on with the standard installation methods. After grabbing the ISO from their [site](https://www.truenas.com/download-truenas-scale/) I wrote it to the drive. I've tried a number of different tools for writing bootable USB drives and have had mixed success most of them. It isn't feature packed but dd is the most reliable for me.

``` sudo dd if=TrueNAS-SCALE-24.iso of=/dev/sdb1 status=progress ```

Once this was done I went ahead and installed the system. I had TrueNAS Core installed on a USB Drive, in core this was supported behavior. While it isn't something you would do on an enterprise system, for a small home NAS it frees up a SATA cable for more drives. During my TrueNAS Scale installation it warned me this was no longer recommended. I'll give it a try anyway and see how it runs, if I need to sacrifice a drive for the OS that's a little annoying but not the end of the world.

### Post-Installation

For today I'm keeping it simple and just getting things up and running again. When you first get back to the WebUI you will be prompted to set new non-default credentials. This good basic security procedure.

From there I had to import my old pools. This is hilariously easy just click import pool and your old pools show up in the drop down list.

![](/assets/screenshots/tnupgrade/2025-02-08_22-31.png)

Next I needed groups and users to authenticate my SMB shares. Under the credentials  I'm using this group just for my SMB share so I named it very simplistically. No special privileges needed and checked the box for SMB Group.

![](/assets/screenshots/tnupgrade/2025-02-08_22-37.png)

Nothing special for the user either, I'm using this for just SMB access. Username/pass and checked the box for SMB User.
![](/assets/screenshots/tnupgrade/2025-02-08_22-39.png)
![](/assets/screenshots/tnupgrade/2025-02-08_22-40.png)

Finally I added a data set for SMB sharing. My pool is migrating from a very old version of FreeNAS that predated the need for datasets. This is a little annoying but it means I have to create datasets and then copy my files into them manually with the CLI. A minor inconvenience overall though. Helpfully when you create a dataset intended as an SMB share it will automatically set it up sharing if you select the SMB Dataset preset. Just a few clicks and it was all working.

![](/assets/screenshots/tnupgrade/2025-02-09_22-56.png)

I went ahead and confirmed the share was working from a windows computer and it was! I had a few hiccups but it was overall a smooth transition from Core to Scale and I'm pretty happy with how it's all running.
