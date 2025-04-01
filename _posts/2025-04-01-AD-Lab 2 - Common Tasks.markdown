---
layout: post
title:  "AD Lab - Common Tasks"
categories: Homelab Projects
---
Working with the AD lab I set up previously, I thought I'd start with some of the most common administrative tasks. Creating users and groups, creating a network share and installing software with a group policy.

### Groups and Users
First I wanted to be able to interact with ADUC in a realistic way. Most of the time administrative tasks are not going to be done directly from the server. I wanted to be able to use the ADUC MMC snap-in from a logged in account. First I created a security group and named it ADmins. In a real security environment you would never want to name something "Admins", it's too similar to default credentials but on my homelab I thought the pun was funny.
![](/assets/screenshots/AD2/2025-03-30_02-48.png)

Next I use the delegation of control wizard to give the ADmins group permissions. In this case I gave the group every permission, but in a real environment you would want to be following the principle of least privilege. 
![](/assets/screenshots/AD2/2025-03-30_02-49.png)![](/assets/screenshots/AD2/2025-03-30_02-50.png)

I add the Admin account I previously created to the group.
![](/assets/screenshots/AD2/2025-03-30_02-51.png)

Finally I install [RSAT](https://www.microsoft.com/en-ca/download/details.aspx?id=45520) on the non-server VM. This is the set of tools that will me to perform administrative duties from a machine other than the server.
![](/assets/screenshots/AD2/2025-03-30_03-56.png)

I check that it's worked, I can now access AD and group policy snap-ins from the account from any VM joined to the domain.
![](/assets/screenshots/AD2/2025-03-30_03-58.png)

I create another user for testing.
![](/assets/screenshots/AD2/2025-03-30_04-04.png)

I test resetting the password and disabling/re-enabling the account. Everything works as expected. Password resets are probably the most commonly performed AD task, disabling accounts is just as important as creating them. Users come and go from the environment, in this case a "disable" is useful as it maintains the data. That user data may need to be accessed in the future.
![](/assets/screenshots/AD2/2025-03-30_04-05.png)

![](/assets/screenshots/AD2/2025-03-30_04-05_1.png)

![](/assets/screenshots/AD2/2025-03-30_04-06.png)

### Installing software with Group Policy

Next I wanted to try installing software with a group policy. I decided to go with a simple piece of commonly used software, Notepad++. Notepad++ is a feature rich but free and open source text editor. It's a fairly realistic example of software a user may request.

First I create an OU called "Notepad fans", people in this organizational unit just really love notepad.  I add my new user Horatio to this group.
![](/assets/screenshots/AD2/2025-03-30_05-38.png)

At first I was thinking I'd just go ahead and make a group policy without a network share, just as a process test. While I was doing this I changed my mind, why not go ahead and make a network share? That'll be easy enough.
![](/assets/screenshots/AD2/2025-03-30_05-49.png)

I make a folder on the server VM and share it, adding permissions. At this point I realize I will also need a security group for the Notepad fans, so I quickly make one and assign Horatio to it before adding it to the share permissions. I give my ADmins write permissions so they can place installation packages in the folder. I drop the notepad++ msi file in this folder.
![](/assets/screenshots/AD2/2025-03-31_03-03.png)

I return to my ADmin account and resume creating the GPO for the software installation. Using the network drive file for Notepad++.
![](/assets/screenshots/AD2/2025-03-30_22-31.png)![](/assets/screenshots/AD2/2025-03-31_01-46.png)

I want this to be installed right away, so I choose Assigned and check the box for "Install this application at Logon". The "published" option makes software optionally available for users. After this I force a gpupdate and give the VM a reboot just to be sure.
![](/assets/screenshots/AD2/2025-03-31_03-55.png)

I log back in on the user account and find Notepad++ has been installed. Success!
![](/assets/screenshots/AD2/2025-03-31_04-00.png)

So far, I find working with AD to be pretty intuitive. These are simple tasks in a simple environment but still highlight the importance of attention to detail. Every permission needs to be properly set for this system to work and remain secure.