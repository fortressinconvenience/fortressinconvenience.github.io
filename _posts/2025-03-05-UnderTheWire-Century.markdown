---
layout: post
title:  "UnderTheWire Century"
categories: Homelab Projects
---
UnderTheWire is another set of wargames intended to teach computer security and CLI competence. UnderTheWire is heavily influenced by the linux based OverTheWire but focuses on powershell instead.  Each level requires SSHing in as a specific user and finding the password for the next user.

This week I completed all the "century" difficulty levels and found it pretty fun. It definitely helped me understand powershell better. As far as I could find, UnderTheWire has no spoiler policy and I will post some of my solutions and thoughts here.

What I found pretty interesting doing this was how many windows CLI commands have aliases for equivalent Linux CLI commands. This was extremely helpful, I could try commands I knew from the Linux CLI and would be able to figure out the windows equivalent with ease.

### Century 1

The first puzzle here was simple, find the version of powershell currently running.

![](/assets/screenshots/UnderTheWire/2025-03-09_03-48.png)

### Century 2

Century 2 is probably easier than century 1, simply find the name of the file on the desktop.

![](/assets/screenshots/UnderTheWire/2025-03-09_03-49.png)
### Century 3

Century 3 got a bit more interesting, it teaches the measure command and it was very helpful through the remaining puzzles. 

![](/assets/screenshots/UnderTheWire/2025-03-09_03-50.png)
### Century 4

Century 4 is simply how to access files and folders with spaces. Even without knowing exact formatting you can use tab autocompletion, this level is a bit of a freebie.
![](/assets/screenshots/UnderTheWire/2025-03-09_03-51.png)
### Century 5

Getting a bit more into windows specifics, this level required the name of the domain it currently resided in. Once you find the command, get-addomain it's easy.

![](/assets/screenshots/UnderTheWire/2025-03-09_03-52.png)

### Century 6

This is just level 3 again but with directories instead of files. A really simple adjustment here solves it.

![](/assets/screenshots/UnderTheWire/2025-03-09_03-54.png)

### Century 7

In powershell ls is an alias for the windows commandlet get-childitem, which can be used to search for specific files. On this level you have to search through all the user folders for a readme file. The trick here is the -recurse flag.

![](/assets/screenshots/UnderTheWire/2025-03-09_03-57.png)

### Century 8

This is where the levels started to get more challenging and interesting. The challenge here is to find the number of unique entries in a text file. In the command I used cat is an alias for get-object and functions pretty similarly to cat. Select-object with the unique flag is pretty similar to the linux command uniq, and measure works the same as in the previous puzzles.

![](/assets/screenshots/UnderTheWire/2025-03-09_03-59.png)

### Century 9
Century 9 is another puzzle where you need to find specific information in a large body of text. This time you're looking for the 161st word. This one took a bit of puzzling for me, first because I didn't realize powershell would format the text itself and second because I wasn't sure if select counted from 0 or 1. 

Ultimately though I used get-object to spit the file on the spaces, with the -raw flag to prevent formatting from powershell. Then select to find the correct word.
![](/assets/screenshots/UnderTheWire/2025-03-03_05-53.png)

### Century 10
This one is a simple problem, find the description of the windows update service using powershell. I'm not sure I found the best solution for this one, I felt like it was pretty complex for what should be a simple task.

After some digging through help pages I found Get-WmiObject win32_service, this did show all of the services in a huge list but without descriptions. I was surprised to find that select-object could be used to select data that was not output by default. This isn't very intuitive, unless you know exactly what you are looking for you don't know if the data is there or not.

Next I tried Get-WmiObject win32_service | select description , this did select the previously not visible descriptions but in a useless format.
![](/assets/screenshots/UnderTheWire/2025-03-04_03-00.png)
Get-WmiObject win32_service | select name,description

Next I selected the name as well, this was a huge improvement but introduced a new problem. The text of the descriptions does not wrap by default, cutting off the full descriptions.
![[2025-03-04_03-00_1.png]]

There's probably a few ways to take that output and format it but I found Get-WmiObject win32_service | select name,description | format-table -wrap . This formatted the text to wrap around allowing me to read the full descriptions and solve the puzzle.
![](/assets/screenshots/UnderTheWire/2025-03-04_03-01.png)

### Century 11
This is another puzzle about finding a specific file out of a large number of documents and files. In this case we knew the file had the hidden attribute but no other information is given. Finding the get-object flags for hidden files it's otherwise the same solution as century 7. On my first use of the command I didn't have the "-erroraction silentlycontinue" and while it still worked the output was littered with permissions errors. I could still find the solution in the messy output but I wanted to find a way to clean it up a bit. Part of the fun here is not just finding a functional solution but hopefully a good solution too.

ls -attributes hidden -recurse -erroraction silentlycontinue 
![](/assets/screenshots/UnderTheWire/2025-03-04_04-46.png)

### Century 12
Century 12 requires you to find the description of the domain controller.

I found the command get-addomaincontroller pretty quickly but couldn't find any description of the domain controller using this command. However I got the name of the domain controller and that's a good start. Even using select I get a blank output. 

![](/assets/screenshots/UnderTheWire/2025-03-04_05-34.png)![](/assets/screenshots/UnderTheWire/2025-03-04_05-38.png)

Next I tried the get-adcomputer commands, specifying the domain controller by name and using select-object to specify the field. get-adcomputer utw | select description. Again the output was blank and this was probably the spot in all of the century levels that stumped me for the longest. Finally I found the -properties flag and that got me the description field I needed.

I found it pretty frustrating that using select to select the description object provides a different description field from the one shown the -properties flag. I spent some times reading after this trying to pinpoint the exact cause of this but didn't find a fully satisfactory answer.

![](/assets/screenshots/UnderTheWire/2025-03-08_21-52.png)![](/assets/screenshots/UnderTheWire/2025-03-08_21-54.png)

### Century 13
The goal of century 13 is to count the number of words in a file. I could really just re-use my solution from Century 9 and actually this problem felt easier to me than Century 9 did. Maybe this tells me that there is a simpler solution for Century 9 and I should go back and try some different methods.

![](/assets/screenshots/UnderTheWire/2025-03-08_22-36.png)

### Century 14
Century 14 extends the same ideas, but this time you need to count reoccurrences of the same word. Quite a bit of added complexity to the same problem. Again I start by splitting on the spaces. I tried a few different solutions before finding the one I was happiest with.

My first successful solution was ($pass = cat ".\Word_File.txt" -raw) -split " " | group | select name, count | sort object -descending . This solution was functional but output 20,000 lines of text. I had to scroll to the top to find the answer, and I had to increase the screen buffer to see it. The obvious solution to this would be to try the -ascending flag with sort-object, but I was surprised to learn it does not have this flag(At least not in this version of powershell). That's a bit bizarre isn't it?

That was a successful answer but outputting 20,000 lines of text and scrolling through them didn't feel like a good way to answer the problem. Sometimes you work with the solution you have but I wanted to find something more functional, this is where I found the where-object command. The syntax for it is a bit complex and when you use the CLI help page for it, it provides you with very little help and give you a URL for the full manual. That's a wee bit silly.

After some fiddling I came up with this final answer that I was much happier with though.
![](/assets/screenshots/UnderTheWire/2025-03-09_01-57.png)
### Century 15
Signing into century 15 only shows you've found the correct solution to Century 14 but this is the end of century levels. I was a little disappointed that there's no congratulatory message in century 15 but I still felt accomplished.
![](/assets/screenshots/UnderTheWire/2025-03-09_01-58.png)

Overall I really enjoyed working on these, but they weren't quite as clever or interesting as the equivalent difficulty OverTheWire levels. The quirks of powershell still provided some interesting challenges though. 