---
layout: post
title:  "OverTheWire - Bandit - 1 to 15"
categories: Homelab Projects
---
[Over The Wire](https://overthewire.org/wargames/) is a website that hosts a collection of wargames designed to teach and test security concepts in a fun, hands-on way. These wargames are essentially a series of challenges that players must overcome by exploiting vulnerabilities or using their knowledge of security principles.

I started working on the "Bandit" levels this week and have had a great time with it. I'm a daily linux user and am pretty familiar with the CLI but (mostly) don't need to engage with the more complex CLI tools. So far none of the bandit levels have given me too much trouble, reading the man pages for the suggested tools gets me through. 

I won't post any level spoilers as it's against the spirit of the game, however there are a few tools I learned about in the first 15 levels I found useful and interesting. I'm mostly writing about them to help me remember them when I do need them next.

### Strings
This tool scans a file for strings of printable text, this is really handy for finding text in large pieces of data that contain mostly machine readable information. Not something I'm going to use everyday but when I need it I hope I remember it.

### Uniq
Another tool for processing large bodies of text, Uniq can find duplicate strings of text, filter, count occurrences and find unique strings. This will be awesome the next time I need to dig through a log file. You can pipe from sort to remove duplicates easily.

### mktemp
mktemp quickly and easily creates temporary directories to work in with unique names. 
I'm surprised I've never heard of this before but I'll definitely use it to speed up my workflow in the future.