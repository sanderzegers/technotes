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





### Access-lists

A simple list based on a prefix consisting of an IPv4 or IPv6 address and netmask or cisco wildcard mask.

Simple netmask which allows 10.10.8.0/24, block everything else:

```
config router access-list
    edit "standard"
        config rule
            edit 1
                set prefix 10.10.8.0 255.255.255.0
            next
        end
    next
end
```

When this access-list is used as a BGP filter, routes that do not match the permit rule are not allowed.

Access-lists can also use a Cisco-style wildcard mask. A wildcard mask defines which bits must match and which bits can be ignored.

A `0` bit means: this bit must match.\
A `1` bit means: this bit can be anything.

```
config router access-list
    edit "cisco_wildcard"
        config rule
            edit 1
                unset prefix
                set wildcard 10.10.8.1 0.0.255.0
            next
            edit 2
                unset prefix
                set wildcard 172.16.0.0 0.0.15.255
            next
        end
    next
end
```

Rule 1 allows, 10.10.<0-255>1

Rule 2 allows, 172.16.<0-15>.<0-255>

