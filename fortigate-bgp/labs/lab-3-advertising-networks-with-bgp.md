# Lab 3: Advertising Networks with BGP

## Topology

<figure><img src="https://github.com/sanderzegers/technotes/raw/refs/heads/undefined/fortigate-bgp/assets/topologies/exports/bgp-lab-master-Lab1.svg" alt=""><figcaption></figcaption></figure>

iBGP Settings:

| Router |    AS | Interface IP | BGP Neighbor | Loopback Lan Network |
| ------ | ----: | ------------ | ------------ | -------------------- |
| R1     | 65001 | 172.18.12.0  | 172.18.12.1  | 10.10.1.1/24         |
| R2     | 65001 | 172.18.12.1  | 172.18.12.0  | 10.10.2.1/24         |

eBGP Settings:

| Router |    AS | Interface IP | BGP Neighbor |
| ------ | ----: | ------------ | ------------ |
| R1     | 65002 | 172.18.12.0  | 172.18.12.1  |
| R2     | 65003 | 172.18.12.1  | 172.18.12.0  |

## Packet Captures

<table><thead><tr><th width="174">PCAP File</th><th>Description</th><th>What to look for</th></tr></thead><tbody><tr><td><a href="https://github.com/sanderzegers/technotes/raw/refs/heads/undefined/fortigate-bgp/pcaps/Lab3/lab3-ebgp-network-announcement.pcapng">lab3-ebgp-network-announcement.pcapng</a></td><td>Announcement via network statement</td><td>BGP UPDATE Message (packet 18)</td></tr><tr><td><a href="https://github.com/sanderzegers/technotes/raw/refs/heads/undefined/fortigate-bgp/pcaps/Lab3/lab3-ibgp-network-announcement.pcapng">lab3-ibgp-network-announcement.pcapng</a></td><td>Announcement via network statement</td><td>BGP UPDATE Message (packet 21)</td></tr><tr><td><a href="https://github.com/sanderzegers/technotes/raw/refs/heads/undefined/fortigate-bgp/pcaps/Lab3/lab3-ibgp-network-removal.pcapng">lab3-ibgp-network-removal.pcapng</a></td><td>Removal Route when interface is going down</td><td>BGP UPDATE Message (packet 29)</td></tr></tbody></table>

## Setup simple network announcements

### iBGP

```
config vdom
edit R1
config router bgp
    set as 65001
    config neighbor
        edit "172.18.12.1"
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
```

```
config vdom
edit R1
config router bgp
    config network
        edit 1
            set prefix 10.10.1.0 255.255.255.0
        next
    end
end
```

```
FGT02 (R2) # get router info routing-table bgp
Routing table for VRF=0
B       10.10.1.0/24 [200/0] via 172.18.12.0 (recursive is directly connected, R1R2-1), 00:07:51, [1/0]
```

```
FGT02 (R2) # get router info bgp summary 

VRF 0 BGP router identifier 172.17.0.2, local AS number 65001
BGP table version is 1
1 BGP AS-PATH entries
0 BGP community entries

Neighbor    V         AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
172.18.12.0 4      65001      13      12        0    0    0 00:08:45        1

Total number of neighbors 1
```

#### Packet analysis

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

R1 sends an UPDATE message which contains:

* ORIGIN: IGP
* AS\_PATH: empty
* NEXT\_HOP: 172.18.12.0
* LOCAL\_PREF: 100
* Network IP and Subnet: 10.10.1.0/24

### eBGP

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

Add network statement for R1 loopback network (10.10.1.0/24)

<pre><code><strong>config vdom
</strong>edit R1
config router bgp
    config network
        edit 1
            set prefix 10.10.1.0 255.255.255.0
        next
    end
end
</code></pre>

Verify the route is received on R2:

```
FGT02 (R2) # get router info bgp summary 

VRF 0 BGP router identifier 172.17.0.2, local AS number 65003
BGP table version is 1
2 BGP AS-PATH entries
0 BGP community entries

Neighbor    V         AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
172.18.12.0 4      65002       5       5        0    0    0 00:02:12        1

Total number of neighbors 1
```

```
FGT02 (R2) # get router info routing-table bgp
Routing table for VRF=0
B       10.10.1.0/24 [20/0] via 172.18.12.0 (recursive is directly connected, R1R2-1), 00:03:22, [1/0]

```

#### Packet analysis

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

R1 sends an UPDATE message which contains:

* ORIGIN: IGP
* AS\_PATH: 65002
* NEXT\_HOP: 172.18.12.0
* Network IP and Subnet: 10.10.1.0/24

## Interface Down

Now let's bring the interface down. This demonstrates that an existing route must exist in the internal routing table for BGP to advertise it.

```
config system interface
    edit "R1-LAN0"
        set status down
    next
end
```

The network is not announced anymore:

```
FGT02 (R1) # sudo R2 get router info bgp summary 

VRF 0 BGP router identifier 172.17.0.2, local AS number 65001
BGP table version is 1
0 BGP AS-PATH entries
0 BGP community entries

Neighbor    V         AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
172.18.12.0 4      65001      26      24        0    0    0 00:19:15        0

Total number of neighbors 1


FGT02 (R1) # sudo R2 get router info routing-table bgp
No route available
```

Now we disable interface check

```
config vdom
edit R1
config router bgp
    config network
        edit 1
            set network-import-check disable
        next
    end
end
next
```

And the route is back again, even though, the loopback interface is still down:

```
FGT02 (R1) # get system interface | grep R1-LAN0
== [ R1-LAN0 ]
name: R1-LAN0   ip: 10.10.1.1 255.255.255.0   status: down    type: loopback   netflow-sampler: disable    sflow-sampler: disable    src-check: enable    mtu-override: disable 

FGT02 (R1) # sudo R2 get router info bgp summary 

VRF 0 BGP router identifier 172.17.0.2, local AS number 65001
BGP table version is 1
1 BGP AS-PATH entries
0 BGP community entries

Neighbor    V         AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
172.18.12.0 4      65001       5       5        0    0    0 00:02:17        1

Total number of neighbors 1


FGT02 (R1) # sudo R2 get router info routing-table bgp
Routing table for VRF=0
B       10.10.1.0/24 [200/0] via 172.18.12.0 (recursive is directly connected, R1R2-1), 00:00:30, [1/0]

```

#### Packet Analysis

Route Removal through UPDATE Message and withdrawn routes option.

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

## Summary

In this lab, BGP was used to advertise a local loopback/LAN prefix with a network statement. The BGP session alone did not advertise the route; the prefix had to be explicitly added to BGP.&#x20;

By default, FortiGate only advertises the prefix if an exact matching route exists in the local routing table. When the interface was disabled, the route was withdrawn. Disabling network-import-check allowed the prefix to be advertised even though the local interface was down.

The iBGP and eBGP UPDATE messages used the same BGP message type, but the path attributes differed. The iBGP advertisement used an empty AS\_PATH and included LOCAL\_PREF, while the eBGP advertisement included the local AS in the AS\_PATH.
