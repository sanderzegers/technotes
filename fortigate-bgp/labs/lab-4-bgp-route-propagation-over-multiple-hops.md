# Lab 4: BGP Route Propagation over Multiple Hops

## Objective

In this lab we demonstrate how BGP advertises networks between two hops and compare iBGP and eBGP behaviour.

## Topology <a href="#topology" id="topology"></a>

<figure><img src="https://github.com/sanderzegers/technotes/raw/refs/heads/undefined/fortigate-bgp/assets/topologies/exports/bgp-lab-master-Lab4.svg" alt=""><figcaption></figcaption></figure>

iBGP Settings:

| Router | AS    | Interface IP                      | BGP Neighbor                      | Loopback Lan Network |
| ------ | ----- | --------------------------------- | --------------------------------- | -------------------- |
| R1     | 65001 | <p>172.18.12.0<br>172.18.13.0</p> | <p>172.18.12.1<br>172.18.13.1</p> | 10.10.1.1/24         |
| R2     | 65001 | 172.18.12.1                       | 172.18.12.0                       | 10.10.2.1/24         |
| R3     | 65001 | 172.18.13.1                       | 172.18.13.0                       |                      |

eBGP Settings:

| Router | AS    | Interface IP                      | BGP Neighbor                      |
| ------ | ----- | --------------------------------- | --------------------------------- |
| R1     | 65002 | <p>172.18.12.0<br>172.18.13.0</p> | <p>172.18.12.1<br>172.18.13.1</p> |
| R2     | 65003 | 172.18.12.1                       | 172.18.12.0                       |
| R3     | 65004 | 172.18.13.1                       | 172.18.13.0                       |

## Packet Captures <a href="#packet-captures" id="packet-captures"></a>

| PCAP File                   | Description                                                                                            | What to look for                                         |
| --------------------------- | ------------------------------------------------------------------------------------------------------ | -------------------------------------------------------- |
| lab4-ebgp-reannounce.pcapng | Two interface capture. Watch how the 10.10.3.0/24 gets forwarded from R3 to R1, and then from R1 to R3 | UPDATE Messages and also AS\_PATH in the UPDATE Messages |
|                             |                                                                                                        |                                                          |
|                             |                                                                                                        |                                                          |

## Route Propagation

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
BGP table version is 1
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

Verify networks are on all routers:

```
FGT02 (R3) # sudo R1 get router info routing-table bgp
Routing table for VRF=0
B       10.10.3.0/24 [20/0] via 172.18.13.1 (recursive is directly connected, R1R3-1-0), 00:00:20, [1/0]


FGT02 (R3) # sudo R2 get router info routing-table bgp
Routing table for VRF=0
B       10.10.3.0/24 [20/0] via 172.18.12.0 (recursive is directly connected, R1R2-1), 00:00:08, [1/0]
```

So network gets re-announced by R1 to R2.

