# Lab 4: BGP Route Propagation over Multiple Hops

## Objective

In this lab, we demonstrate how BGP advertises networks over two hops and compare iBGP and eBGP behaviour.

## Topology <a href="#topology" id="topology"></a>

<figure><img src="https://github.com/sanderzegers/technotes/raw/refs/heads/undefined/fortigate-bgp/assets/topologies/exports/bgp-lab-master-Lab4.svg" alt=""><figcaption></figcaption></figure>

iBGP Settings:

| Router | AS    | Interface IP                      | BGP Neighbor                      | Loopback Lan Network |
| ------ | ----- | --------------------------------- | --------------------------------- | -------------------- |
| R1     | 65001 | <p>172.18.12.0<br>172.18.13.0</p> | <p>172.18.12.1<br>172.18.13.1</p> | 10.10.1.1/24         |
| R2     | 65001 | 172.18.12.1                       | 172.18.12.0                       | 10.10.2.1/24         |
| R3     | 65001 | 172.18.13.1                       | 172.18.13.0                       | 10.10.3.1/24         |

eBGP Settings:

| Router | AS    | Interface IP                      | BGP Neighbor                      |
| ------ | ----- | --------------------------------- | --------------------------------- |
| R1     | 65002 | <p>172.18.12.0<br>172.18.13.0</p> | <p>172.18.12.1<br>172.18.13.1</p> |
| R2     | 65003 | 172.18.12.1                       | 172.18.12.0                       |
| R3     | 65004 | 172.18.13.1                       | 172.18.13.0                       |

## Packet Captures <a href="#packet-captures" id="packet-captures"></a>

| PCAP File                                                                                                                                              | Description                                                                                                     | What to look for                                          |
| ------------------------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------- |
| [lab4-ebgp-reannounce.pcapng](https://github.com/sanderzegers/technotes/raw/refs/heads/undefined/fortigate-bgp/pcaps/Lab4/lab4-ebgp-reannounce.pcapng) | A dual interface capture on R1. BGP connection established, and route announcement and forward                  | UPDATE Messages and AS\_PATH in the UPDATE Messages       |
| [lab4-ibgp-reannounce.pcapng](https://github.com/sanderzegers/technotes/raw/refs/heads/undefined/fortigate-bgp/pcaps/Lab4/lab4-ibgp-reannounce.pcapng) | A dual interface capture on R1. This time the route is only announced from R3 to R1, R1 does not announce to R2 | Single UPDATE Message and AS\_PATH in the UPDATE Messages |

## Route Propagation with eBGP

Setup Basic eBGP peering

```
config vdom
edit R1
config router bgp
    set as 65002
    config neighbor
        edit "172.18.12.1"
            set remote-as 65003
        next
        edit "172.18.13.1"
            set remote-as 65004
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
next
edit R3
config router bgp
    set as 65004
    config neighbor
        edit "172.18.13.0"
            set remote-as 65002
        next
    end
end
```

Verify BGP connectivity:

```
FGT02 (R1) # get router info bgp summary 

VRF 0 BGP router identifier 172.17.0.1, local AS number 65002
BGP table version is 1``
0 BGP AS-PATH entries
0 BGP community entries

Neighbor    V         AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
172.18.12.1 4      65003       2       3        0    0    0 00:00:03        0
172.18.13.1 4      65004       2       2        0    0    0 00:00:02        0

Total number of neighbors 2
```

Two neighbors are connected on R1. Remote AS 65003 and 65004. No prefixes announced yet.

Let's announce 10.10.3.0/24 on R3 and verify which routers receive the routes.

```
config vdom
edit R3
config router bgp
config network
edit 0
set prefix 10.10.3.0/24
next
end
end
```

Verify the 10.10.3.0/24 networks is installed on all routers:

```
FGT02 (R3) # sudo R1 get router info routing-table bgp
Routing table for VRF=0
B       10.10.3.0/24 [20/0] via 172.18.13.1 (recursive is directly connected, R1R3-1-0), 00:00:20, [1/0]


FGT02 (R3) # sudo R2 get router info routing-table bgp
Routing table for VRF=0
B       10.10.3.0/24 [20/0] via 172.18.12.0 (recursive is directly connected, R1R2-1), 00:00:08, [1/0]
```

So the network got re-announced by R1 to R2. Mind you there is no direct peering between R1 and R3

From a received and advertised-routes perspective:

```
FGT02 (R3) # sudo R3 get router info bgp neighbors 172.18.13.0 advertised-routes 
VRF 0 BGP table version is 1, local router ID is 172.17.0.3
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal
Origin codes: i - IGP, e - EGP, ? - incomplete

   Network          Next Hop            Metric     LocPrf Weight RouteTag Path
*> 10.10.3.0/24     172.18.13.1                   100  32768        0 i <-/->

Total number of prefixes 1
```

Received on R1:

```
FGT02 (R3) # sudo R1 get router info bgp neighbors 172.18.13.1 routes
VRF 0 BGP table version is 1, local router ID is 172.17.0.1
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              S Stale
Origin codes: i - IGP, e - EGP, ? - incomplete

   Network          Next Hop            Metric     LocPrf Weight RouteTag Path
*> 10.10.3.0/24     172.18.13.1     0                      0        0 65004 i <-/1>

Total number of prefixes 1
```

Forwarded to R2:

```
FGT02 (R3) # sudo R1 get router info bgp neighbors 172.18.12.1 advertised-routes
VRF 0 BGP table version is 1, local router ID is 172.17.0.1
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal
Origin codes: i - IGP, e - EGP, ? - incomplete

   Network          Next Hop            Metric     LocPrf Weight RouteTag Path
*> 10.10.3.0/24     172.18.12.0                            0        0 65004 i <-/->

Total number of prefixes 1
```

Now check the routes received on R2. Note that the AS\_PATH has grown. To reach 10.10.3.0/24 in AS 65004, traffic must transit through AS 65002.

```
FGT02 (R3) # sudo R2 get router info bgp neighbors 172.18.12.0 routes
VRF 0 BGP table version is 1, local router ID is 172.17.0.2
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              S Stale
Origin codes: i - IGP, e - EGP, ? - incomplete

   Network          Next Hop            Metric     LocPrf Weight RouteTag Path
*> 10.10.3.0/24     172.18.12.0     0                      0        0 65002 65004 i <-/1>

Total number of prefixes 1
```

## Route Propagation with iBGP

Now let's start with a clean lab, and configure iBGP peering, between R1-R2 and R1-R3.

```
config vdom
edit R1
config router bgp
    set as 65001
    config neighbor
        edit "172.18.12.1"
            set remote-as 65001
        next
        edit "172.18.13.1"
            set remote-as 65001
        next
    end
end
next
edit R2
config router bgp
    set as 65001
    config neighbor
        edit "172.18.12.0"
            set remote-as 65001
        next
    end
end
next
edit R3
config router bgp
    set as 65001
    config neighbor
        edit "172.18.13.0"
            set remote-as 65001
        next
    end
end
```

Verify BGP connectivity on R1:

```
FGT02 (R1) # get router info bgp summary

VRF 0 BGP router identifier 172.17.0.1, local AS number 65001
BGP table version is 1
0 BGP AS-PATH entries
0 BGP community entries

Neighbor    V         AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
172.18.12.1 4      65001       2       3        0    0    0 00:00:33        0
172.18.13.1 4      65001       4       5        0    0    0 00:01:48        0

Total number of neighbors 2
```

Now just like we did in the eBGP lab, we'll announce the local net (10.10.3.0/24) on R3:

```
config vdom
edit R3
config router bgp
config network
edit 0
set prefix 10.10.3.0/24
next
end
end
```

Verify received networks on all routers:

```
FGT02 (R3) # sudo R1 get router info routing-table bgp
Routing table for VRF=0
B       10.10.3.0/24 [200/0] via 172.18.13.1 (recursive is directly connected, R1R3-1-0), 00:00:48, [1/0]


FGT02 (R3) # sudo R2 get router info routing-table bgp
No route available

```

10.10.3.0/24 is not advertised to R2. By default, iBGP does not advertise routes learned from other iBGP peers.

R3 announces 10.10.3.0/24 to R1:

```
FGT02 (global) # sudo R3 get router info bgp neighbors 172.18.13.0 advertised-routes 
VRF 0 BGP table version is 1, local router ID is 172.17.0.3
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal
Origin codes: i - IGP, e - EGP, ? - incomplete

   Network          Next Hop            Metric     LocPrf Weight RouteTag Path
*>i10.10.3.0/24     172.18.13.1                   100  32768        0 i <-/->

Total number of prefixes 1
```

Which is also received by R1:

```
FGT02 (global) # sudo R1 get router info bgp neighbors 172.18.13.1 routes
VRF 0 BGP table version is 1, local router ID is 172.17.0.1
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              S Stale
Origin codes: i - IGP, e - EGP, ? - incomplete

   Network          Next Hop            Metric     LocPrf Weight RouteTag Path
*>i10.10.3.0/24     172.18.13.1     0             100      0        0 i <-/1>

Total number of prefixes 1
```

But R1 does not advertise the 10.10.3.0/24 route to R2:

```
FGT02 (global) # sudo R1 get router info bgp neighbors 172.18.12.1 advertised-routes
% No prefix for neighbor 172.18.12.1
```

## Summary

In this lab, we extended the topology from two routers to three routers. With eBGP, R2 can advertise a route learned from R1 to R3, and the AS\_PATH is updated as the route crosses autonomous systems.&#x20;

With iBGP, R2 learns the route from R1, but does not advertise that iBGP-learned route to R3 by default. This behavior prevents routing loops inside an AS and is the reason larger iBGP networks require either a full mesh or route reflectors.
