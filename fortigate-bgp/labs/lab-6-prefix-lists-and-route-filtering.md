# Lab 6: Prefix-lists and route filtering

## Objective

In this lab we learn how to control BGP route advertisements. We start by advertising multiple networks, then use prefix-lists to allow or block specific routes. Finally, we introduce route-maps as a more flexible way to match and control BGP routes.

This lab starts with a short introduction to prefix-lists.

## Topology

<figure><img src="https://github.com/sanderzegers/technotes/raw/refs/heads/undefined/fortigate-bgp/assets/topologies/exports/bgp-lab-master-Lab1.svg" alt=""><figcaption></figcaption></figure>

| Router |    AS | Interface IP | BGP Neighbor | Local Networks                       |
| ------ | ----: | ------------ | ------------ | ------------------------------------ |
| R1     | 65002 | 172.18.12.0  | 172.18.12.1  | <p>10.10.1.0/24<br>10.10.11.0/24</p> |
| R2     | 65003 | 172.18.12.1  | 172.18.12.0  | 10.10.2.1/24                         |

## Packet Captures

<table><thead><tr><th width="174">PCAP File</th><th>Description</th><th>What to look for</th></tr></thead><tbody><tr><td></td><td></td><td></td></tr></tbody></table>

## Prefix-list

The Prefix-list is used to match network prefixes. This is usually the cleanest option for BGP route filtering.&#x20;

You can see a simple prefix-list example below. Note The default action for any non specified prefix is deny.

```
config router prefix-list    
    edit "route-in"
        config rule
            edit 1
                set action permit
                set prefix 10.10.1.0 255.255.255.0
            next
        end
    end
end
```

Optionally the prefix ranges can be broaden by using the `ge` and `le` parameters.

```
config router prefix-list
    edit "route-in"
        config rule
            edit 1
                set action permit
                set prefix 10.10.1.0 255.255.255.0
                set ge 25
                set le 27
            next
        end
    end
end
```

This needs some additional explanation.&#x20;

`ge` stands for greater or equals.\
In the example below, the subnet mask is set for the prefix is set to /24.

`ge 25` means prefix length **greater than or equal to 25**.\
&#x20;/25, /26, /27, /28, /29, /l30, /31 , /32

`le 27` means prefix length **less than or equal to 27**.\
&#x20;/27, /26, /25, /24

Combined together this means /25, /26, /27.

So this prefix-list allows subnets inside `10.10.1.0/24` with a prefix length from `/25` to `/27`.

Some examples:

```
10.10.1.0/25
10.10.1.128/25
10.10.1.0/26
10.10.1.64/26
10.10.1.0/27
10.10.1.32/27
```



## Outbound prefix-list

We'll start with the eBGP peering from Lab2. Re-Load the lab baseline and execute:

We'll add an additional loopback interface to R1

```
config vdom
edit R1
config system interface
    edit "R1-LAN1"
        set vdom "R1"
        set ip 10.10.11.1 255.255.255.0
        set allowaccess ping
        set type loopback
    next
end
end
```

Next we'll configure the peering between R1 and R2 and announce both LAN IP Ranges

```
config vdom
edit R1
config router bgp
    set as 65002
    config neighbor
        edit "172.18.12.1"
            set remote-as 65003
        next
    end
      
end
next
edit R2
config router bgp
    set as 65003
    config neighbor
        edit "172.18.12.0"
            set remote-as 65002
        next
    end
end
```

Verify that R2 received both routes:

```
BGP-LAB (R2) # get router info bgp sum

VRF 0 BGP router identifier 172.17.0.2, local AS number 65003
BGP table version is 1
2 BGP AS-PATH entries
0 BGP community entries

Neighbor    V         AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
172.18.12.0 4      65002       6       4        0    0    0 00:02:11        2

Total number of neighbors 1


BGP-LAB (R2) # get router info routing-table bgp
Routing table for VRF=0
B       10.10.1.0/24 [20/0] via 172.18.12.0 (recursive is directly connected, R1R2-1), 00:01:05, [1/0]
B       10.10.11.0/24 [20/0] via 172.18.12.0 (recursive is directly connected, R1R2-1), 00:00:36, [1/0]

```

Now let's make sure R1 only announces 10.10.1.0/24 by creating a prefix-list and apply it on the neighbor on R1.

Create a prefix list, and assign to the neighbor:

```
config vdom
edit R1
config router prefix-list
    edit "only_10.10.11.0"
        config rule
            edit 1
                set prefix 10.10.11.0 255.255.255.0
                unset ge
                unset le
            next
        end
    next
end
config router bgp
    set as 65002
    config neighbor
        edit "172.18.12.1"
            set prefix-list-out "only_10.10.11.0"
        next
    end
end
```

Now let's check the advertised routes from R1 to R2:

```

BGP-LAB (R1) # get router info bgp neighbors 172.18.12.1 advertised-routes 
VRF 0 BGP table version is 1, local router ID is 172.17.0.1
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal
Origin codes: i - IGP, e - EGP, ? - incomplete

   Network          Next Hop            Metric     LocPrf Weight RouteTag Path
*> 10.10.1.0/24     172.18.12.0                   100  32768        0 i <-/->
*> 10.10.11.0/24    172.18.12.0                   100  32768        0 i <-/->

Total number of prefixes 2

```

Still both routes! When applying or changing prefix-lists or route maps. BGP needs to be resetted. We'll do a soft reset for the outgoing routes to 172.18.12.1, to minimize the impact.

```
BGP-LAB (R1) # execute router clear bgp ip 172.18.12.1 soft out
```

Now we wait for the next update timer. And we should see that only 10.10.11.0/24 is announced.

```
BGP-LAB (R1) # get router info bgp neighbors 172.18.12.1 advertised-routes
VRF 0 BGP table version is 1, local router ID is 172.17.0.1
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal
Origin codes: i - IGP, e - EGP, ? - incomplete

   Network          Next Hop            Metric     LocPrf Weight RouteTag Path
*> 10.10.11.0/24    172.18.12.0                   100  32768        0 i <-/->

Total number of prefixes 1
```

```
BGP-LAB (R1) # sudo R2 get router info routing-table bgp
Routing table for VRF=0
B       10.10.11.0/24 [20/0] via 172.18.12.0 (recursive is directly connected, R1R2-1), 1d04h56m, [1/0]

```

Now let's remove the outgoing prefix-list on R1, and add the same prefix-list on R2 and apply as an incoming prefix-list.

```
config vdom
edit R2
config router prefix-list
    edit "only_10.10.11.0"
        config rule
            edit 1
                set prefix 10.10.11.0 255.255.255.0
                unset ge
                unset le
            next
        end
    next
end
config router bgp
    config neighbor
        edit "172.18.12.0"
            set prefix-list-in "only_10.10.11.0"
        next
    end
end
next
edit R1
config router bgp
    config neighbor
        edit "172.18.12.1"
            unset prefix-list-out
        next
    end
end
```

Now lets reset BGP peering on both sites:

```
BGP-LAB (R1) # execute router clear bgp all

BGP-LAB (R1) # sudo R2 execute router clear bgp all
```

Now verify the advertise and received routes

```
BGP-LAB (R2) # sudo R1 get router info bgp neighbors 172.18.12.1 advertised-routes 
VRF 0 BGP table version is 1, local router ID is 172.17.0.1
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal
Origin codes: i - IGP, e - EGP, ? - incomplete

   Network          Next Hop            Metric     LocPrf Weight RouteTag Path
*> 10.10.1.0/24     172.18.12.0                   100  32768        0 i <-/->
*> 10.10.11.0/24    172.18.12.0                   100  32768        0 i <-/->

Total number of prefixes 2


BGP-LAB (R2) # get router info bgp neighbors 172.18.12.0 routes 
VRF 0 BGP table version is 1, local router ID is 172.17.0.2
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              S Stale
Origin codes: i - IGP, e - EGP, ? - incomplete

   Network          Next Hop            Metric     LocPrf Weight RouteTag Path
*> 10.10.11.0/24    172.18.12.0     0                      0        0 65002 i <-/1>

Total number of prefixes 1
```

## Links

{% embed url="https://community.fortinet.com/fortigate-3/technical-tip-how-to-control-bgp-route-advertisement-with-prefix-list-95213" %}

{% embed url="https://community.fortinet.com/fortigate-3/technical-tip-how-to-combine-operators-ge-and-le-in-prefix-list-for-route-map-for-filtering-bgp-routes-104051" %}

{% embed url="https://docs.fortinet.com/document/fortigate/8.0.0/administration-guide/315077/access-lists" %}
