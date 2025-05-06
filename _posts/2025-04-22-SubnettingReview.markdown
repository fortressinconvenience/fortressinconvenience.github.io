---
layout: post
title:  "Subnetting Review"
categories: Homelab Projects
---
Unfortunately I'm not in a position where I get to subnet on a daily basis and certainly not in volume. Subnetting is one of those fundamental IT skills and one a lot of people struggle with. While it comes back to me easily with some practice I do feel like it's helpful to review fundamental topics from time to time.

### What is a subnet and why?
In the simplest terms a subnet is a subdivision of a network. Maybe the name gives away what it does but why it exists?

Subnetting was implemented along with Classless Interdomain Routing as a solution to two problems. The first was preventing the exhaustion of IPv4 addresses(Still a big issue today). The second is reducing the size of routing tables by allowing them to be hierarchical.

That pertains to the routing and organization of the internet at large. Subnetting on a local network gives you some of the same advantages. A hierarchical structure allows for logical grouping of internal addresses. This network segmentation is great for security, administration and helps prevent broadcast storms.

### Subnet Masks
A subnet mask and an IP address both are a 32 bit binary value. With the IP address represented in binary an AND operation is performed. This operation reveals how the IP address is being split up for use by the network.

### Subnetting, the chart method
There are quite a few commonly used methods for quickly calculating subnets but this one is my favourite. You create two simple charts with a pen and paper and use these to aid your calculation. This works great in exams as well.

The first chart is for quick conversion from CIDR notation. 

``` 
/1  /9   /17  /25  128  2    128
/2  /10  /18  /26  192  4    64
/3  /11  /19  /27  224  8    32
/4  /12  /26  /28  240  16   16
/5  /13  /21  /29  248  32   8
/6  /14  /22  /30  252  64   4
/7  /15  /23  /31  254  128  2
/8  /16  /24  /32  255  256  1
```

The second chart required is for quickly locating which subnet the address is in, this one is slightly more annoying to write out but both can still be done in a few minutes once you understand them. I've broken the second chart up into 4 code blocks here to prevent wrapping, if you're typing this out on the exam whiteboard of pearson vue or something similar you may have to do this as well. You can quickly see the patterns here which is why it's so quick to make.

```
128
64                                                64
32                        32                      64          
16            16          32          48          64          80
8        8    16    24    32    40    48    56    64    72    80
4    0 4 8 12 16 20 24 28 32 36 40 44 48 52 56 60 64 68 72 76 80 84 
```

```
128                                      128
64                                       128
32        96                             128          
16        96             112             128             144
8   88    96     104     112     120     128     136     144     152
4   88 92 96 100 104 108 112 116 120 124 128 132 136 140 144 148 152
```

```
128       
64       160                             192      
32       160             176             192             208    
16       160     168     176     184     192     200     208     216 
4    156 160 164 168 172 176 180 184 188 192 196 200 204 208 212 216
```

```
128
64      224
32      224             240
16      224     232     240     248
4   220 224 228 232 236 240 244 248
```

### Using the charts


Starting with an easier example: 219.222.173.130/27

We grab the /27 and find it on the first chart:

```
/1  /9   /17  /25  128  2    128
/2  /10  /18  /26  192  4    64
/3  /11  /19  /27  224  8    32
/4  /12  /26  /28  240  16   16
/5  /13  /21  /29  248  32   8
/6  /14  /22  /30  252  64   4
/7  /15  /23  /31  254  128  2
/8  /16  /24  /32  255  256  1
```

This gives us 224. /27 is in the 4th column, indicating that the 224 is the 4th octet. All others are 255.

This gives a subnet mask of 255.255.255.224. Now take the address and place it over the subnet mask. If the mask is 255, bring down the address octet. If it's 0, bring down 0. For the final octet which is neither 255 or 0, reference the last column of the first chart. Take that 32 and apply it to the second chart and find where the 130 would fit on that line. It fits in the 128 block, so 128 is the final octet.

The Broadcast is the next line on the same chart -1(or 255 otherwise), the first and last useable IPs are the Network add +1 and the broadcast address -1.

Address:      219.222.173.130
Mask:          255.255.255.224
Net Add:     219.222.173.128
Broadcast:   219.222.173.159


### Example 2
33.135.176.29/17

Here's application with a slightly trickier one. The first chart for /17 gives us 128 and the /17 is the third column. The netmask is 255.255.128.0. On the second chart this puts us in the second block of 128 addresses. This makes the network address 33.135.128.0

Address:    33.135.176.29
Mask:        255.255.128.0
Net add:    33.135.128.0
Broadcast: 33.135.255.254


### Final thoughts
This method feels a bit silly, but is very fast. Anyone with this chart can subnet as fast as someone with years of practice. The more you practice, the less you'll need the charts but it's always nice to be able to double check your work.
