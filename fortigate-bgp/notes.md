# Notes

External BGP: Between AS\
Internal BGP: Within AS

Range 1 – 64511 are globally unique AS numbers\
Range 64512 – 65535 are private autonomous system numbers.

BGP is a path vector routing protocol

It stores the path we have to go through to reach the destination network

Just a small note on timers: the timers configured on the FortiGate are not exact delays. RFC 4271 explicitly allows jitter to be applied to BGP timers to prevent peers from sending updates in synchronized bursts. The RFC suggests using a random factor between 0.75 and 1.0 of the configured timer value.



## BGP introductionary courses and sources

Books:

Internet Routing Architectures (Second Edition) Sam Halabi

Online Courses:

[https://networklessons.com/bgp](https://networklessons.com/bgp)

eBGP simulation:

{% embed url="https://nsg.ee.ethz.ch/blog/2020-04-14_mini_internet/" %}

{% embed url="https://github.com/nsg-ethz/mini_internet_project?utm_source=chatgpt.com" %}

{% embed url="https://dn42.dev/services/Route-Collector?utm_source=chatgpt.com" %}

## Lab draft

Part 1: BGP Peering Basics\
Lab 1: Basic iBGP Peering\
Lab 2: Basic eBGP Peering\
Lab 3: Comparing iBGP and eBGP Neighbor Output

Part 2: Route Advertisement\
Lab 4: Advertising Networks\
Lab 5: Comparing AS Path and Next-Hop Behavior

Part 3: iBGP-Specific Behavior\
Lab 6: iBGP Split-Horizon\
Lab 7: Route Reflectors\
Lab 8: next-hop-self

Part 4: eBGP-Specific Behavior\
Lab 9: eBGP Multihop\
Lab 10: AS Path Prepending\
Lab 11: Basic Transit Between ASes

Part 5: Common BGP Tools\
Lab 12: Prefix Filtering\
Lab 13: Route Maps\
Lab 14: Communities\
Lab 15: Soft Reset and Route Refresh

| Lab                                     | Routers | Reason                                                   |
| --------------------------------------- | ------: | -------------------------------------------------------- |
| Lab 6: Prefix-lists and route filtering |       2 | Simple and focused                                       |
| Lab 7: Path selection                   |  3 or 4 | Needs multiple possible paths                            |
| Lab 8: Route-maps and attributes        |  3 or 4 | Better with multiple paths                               |
| Lab 9: Redistribution                   |       3 | Nice to show connected/static/OSPF-to-BGP-style behavior |
