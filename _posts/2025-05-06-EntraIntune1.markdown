---
layout: post
title:  "Entra and Intune - Creating an account and joining a device"
categories: Homelab Projects
---
I've previously set up a small virtual lab for Active Directory and found it pretty informative but using Active Directory for management is on the decline. Microsoft wants everyone moving over to management entirely with EntraID(Previous Azure AD) and Intune. This moves the functions of AD into the cloud and includes mobile device management as well. There's always some annoyances(Microsoft loves changing things) but overall I'd say it seems pretty convenient.

The first big problem to solve is how to get access to these tools for the purposes of a lab. After some digging it turns out that the easiest option is to set up a free trial of Microsoft 365 business. You do need to enter payment information but they don't charge for the first 30 days. If you make a new email you can do this as many times as you need for testing. Microsoft's learning resources are pretty good overall, so it's a shame you have to do this for testing Entra and Intune.

Once I got my Microsoft business account up and running I wanted to try completing a few of the same tasks I had done recently in an AD lab. Adding a user, joining a device and enforcing a simple desktop background policy.

I started in the Entra admin portal and created a new user.
![](/assets/screenshots/entra1/2025-05-05_02-56.png)

Next I assigned this user a license. I also tried the dark mode version of the portal as you can see in the screenshot here.
![](/assets/screenshots/entra1/2025-05-05_03-05.png)

I then spun up another VM, this time Windows 11 Enterprise Evaluation and joined it to Entra.
![](/assets/screenshots/entra1/2025-05-05_02-58.png)

A mild annoyance here, Microsoft is enforcing 2fa on all accounts. This is the right choice from a security perspective but when you're making accounts just for a lab it's quite unwieldly. Anyway I set up 2fa and kept on trucking.
![](/assets/screenshots/entra1/2025-05-05_03-00.png)![](/assets/screenshots/entra1/2025-05-05_03-50.png)

All signed in, everything so far appears to be working.

![](/assets/screenshots/entra1/2025-05-05_03-54.png)
Confirming the device registered.
![](/assets/screenshots/entra1/2025-05-05_03-55.png)

Next I gave Gerald a role and made a group for managers. I made Gerald a manager and set up the group to assign the Global Admin role to all group members.
![](/assets/screenshots/entra1/2025-05-05_04-03.png)![](/assets/screenshots/entra1/2025-05-05_04-05.png)

Finally I wanted to create a policy that enforced a desktop background, I started using the existing templates.
![](/assets/screenshots/entra1/2025-05-05_04-45.png)![](/assets/screenshots/entra1/2025-05-05_04-46.png)

I found a good generic background image and uploaded it to imgur, but you'd probably want to use a network share for this in a real configuration.
![](/assets/screenshots/entra1/2025-05-05_04-49.png)

A assigned the rule to all devices for the moment.
![](/assets/screenshots/entra1/2025-05-05_22-38.png)

This is when I encountered a small issue, I decided to apply the rule only to specific versions of Windows. You can't see it here but in the "value" box but I've selected multiple versions of windows. The issue I encountered here was that the Enterprise Evaluation version of windows 11 wasn't encompassed by the Win 11 Enterprise setting. The first time I applied this rule it showed "not applicable". I think most people would assume the evaluation version was included in "Windows 11 Enterprise", but there you go.
![](/assets/screenshots/entra1/2025-05-05_04-51.png)

After a little bit of troubleshooting, it works! 
![](/assets/screenshots/entra1/2025-05-06_00-53.png)

Entra and Intune are pretty convenient and I can see why Microsoft is pushing for this change. Next up I'll probably try doing some of the same activities again but using powershell or some mobile device management.
