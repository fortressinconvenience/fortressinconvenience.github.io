---
layout: post
title:  "OverTheWire - Bandit - 16 to 33"
categories: Homelab Projects
---
[Over The Wire](https://overthewire.org/wargames/) is a website that hosts a collection of wargames designed to teach and test security concepts in a fun, hands-on way. These wargames are essentially a series of challenges that players must overcome by exploiting vulnerabilities or using their knowledge of security principles.

I've been continuing to work on the Bandit levels when I had free time and finally finished all current levels. The final half was pretty interesting, a few very challenging levels and a number of pretty easy ones. Roughly 4 were simple git commands, which is useful but felt a little out of place. 

![](/assets/screenshots/pfsenseVLAN/2025-02-23_06-04.png)

My favourite level and ultimately the most challenging was level 25, this took some serious lateral thinking and took me by far the longest to solve. The solution is simple but extremely unconventional and I wish they had provided another hint or two.

If you've encountered this because of my mention of level 25, I will not spoil the solution but I will provide a hint in the next section:

Think about the "more" command that is being executed when you log into bandit26. Consider why it operates the way it does and how you can influence it's behaviour. Consider what tools you are able to access from within "more" and how you are able to use them. The solution may not work in some terminal emulators.

Again a few tools I found interesting and useful while exploring these levels.
### diff
The "diff" tool in Linux is a command-line utility that compares two files and displays the differences between them. It's an essential tool for developers, system administrators, and anyone who needs to track changes in text files.
### nc
Netcat, or "nc," is a highly versatile command-line networking utility. It allows users to create and manage both TCP and UDP network connections, acting as either a client or a server. I've used nc a few times in the past (lightly) but had some interesting uses for it in these challenges too. I'm a little startled by how much this one utility can do. 

I often struggle to memorize commands, especially when I use them infrequently I feel like I'm always looking them up again. I really had fun with the puzzles presented by overthewire and found they helped me hammer some of those commands into my memory. Big thank you to overthewire! I'm not sure if I'll move on to trying some of the harder levels or try the windows based underthewire next. Powershell is a little daunting with that weird syntax, isn't it?