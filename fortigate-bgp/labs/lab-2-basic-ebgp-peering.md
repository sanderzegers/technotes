# Lab 2: Basic eBGP Peering



## Objective

This lab repeats the Lab 1 workflow, this time the routers are placed in different autonomous systems. The goal is to compare iBGP and eBGP session establishment.

## Topology

<figure><img src="https://github.com/sanderzegers/technotes/raw/refs/heads/undefined/fortigate-bgp/assets/topologies/exports/bgp-lab-master-Lab1.svg" alt=""><figcaption></figcaption></figure>

| Router |    AS | Interface IP | BGP Neighbor |
| ------ | ----: | ------------ | ------------ |
| R1     | 65002 | 172.18.12.0  | 172.18.12.1  |
| R2     | 65003 | 172.18.12.1  | 172.18.12.0  |

## Packet Captures

<table><thead><tr><th width="174">PCAP File</th><th>Description</th><th>What to look for</th></tr></thead><tbody><tr><td><a href="https://github.com/sanderzegers/technotes/raw/refs/heads/undefined/fortigate-bgp/pcaps/Lab2/lab1-bgp-simple-ebgp-peer.pcapng">lab2-bgp-simple-ebgp-peer.pcapng</a></td><td>Very simple peering, without route announcements. Keep-alive send every 50 secs, with default configuration.</td><td></td></tr></tbody></table>

## Peering requirements

To create an ebgp peering between both peers, the same conditions must match.

| Requirement         | Description                                                                                           |
| ------------------- | ----------------------------------------------------------------------------------------------------- |
| IP reachability     | Each peer must be able to reach other peer's BGP address                                              |
| TCP/179 Allowed     | BGP uses TCP Port 179. If traffic is passing a firewall, traffic must be allowed by a firewall policy |
| Correct Remote AS   | Each router must configure the other router's AS correctly. For iBGP, this is the same  AS            |
| Correct neighbor IP | The configured neighbor address must match the source address used by the peer                        |
| Router ID           | Each BGP router should have a unique router ID                                                        |



## 1st Successful peering

Minimal setup. Define the local AS and configure the neighbor with the remote peer’s AS.

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

### Verify BGP peering summary

Note the different local AS and remote AS

```
FGT02 (R1) # get router info bgp  summary 

VRF 0 BGP router identifier 172.17.0.1, local AS number 65002
BGP table version is 1
0 BGP AS-PATH entries
0 BGP community entries

Neighbor    V         AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
172.18.12.1 4      65003       2       2        0    0    0 00:00:08        0

Total number of neighbors 1

```

### Verify BGP Neighbor Status

`get router info bgp neighbors <ip>`&#x20;

```
FGT02 (R1) # get router info bgp neighbors 172.18.12.1

VRF 0 neighbor table:
BGP neighbor is 172.18.12.1, remote AS 65003, local AS 65002, external link
  BGP version 4, remote router ID 172.17.0.2
  BGP state = Established, up for 00:01:48
  Last read 00:00:04, hold time is 180, keepalive interval is 40 seconds
  Configured hold time is 180, keepalive interval is 40 seconds
  Neighbor capabilities:
    Route refresh: advertised and received (old and new)
    Address family IPv4 Unicast: advertised and received
    Address family VPNv4 Unicast: advertised and received
    Address family IPv6 Unicast: advertised and received
    Address family VPNv6 Unicast: advertised and received
    Address family L2VPN EVPN: advertised and received
  Received 4 messages, 0 notifications, 0 in queue
  Sent 5 messages, 0 notifications, 0 in queue
  Route refresh request: received 0, sent 0
  NLRI treated as withdraw: 0
  Minimum time between advertisement runs is 30 seconds

 For address family: IPv4 Unicast
  BGP table version 1, neighbor version 0
  Index 1, Offset 0, Mask 0x2
  Community attribute sent to this neighbor (both)
  0 accepted prefixes, 0 prefixes in rib
  0 announced prefixes

 For address family: VPNv4 Unicast
  BGP table version 1, neighbor version 0
  Index 1, Offset 0, Mask 0x2
  Community attribute sent to this neighbor (both)
  0 accepted prefixes, 0 prefixes in rib
  0 announced prefixes

 For address family: IPv6 Unicast
  BGP table version 1, neighbor version 0
  Index 1, Offset 0, Mask 0x2
  Community attribute sent to this neighbor (both)
  0 accepted prefixes, 0 prefixes in rib
  0 announced prefixes

 For address family: VPNv6 Unicast
  BGP table version 1, neighbor version 0
  Index 1, Offset 0, Mask 0x2
  Community attribute sent to this neighbor (both)
  0 accepted prefixes, 0 prefixes in rib
  0 announced prefixes

 For address family: L2VPN EVPN
  BGP table version 1, neighbor version 0
  Index 1, Offset 0, Mask 0x2
  Community attribute sent to this neighbor (both)
  0 accepted prefixes, 0 prefixes in rib
  0 announced prefixes

 Connections established 1; dropped 0
Local host: 172.18.12.0, Local port: 17151
Foreign host: 172.18.12.1, Foreign port: 179
Egress interface: 58
Nexthop: 172.18.12.0
Nexthop interface: R1R2-0
Nexthop global: ::
Nexthop local: ::
BGP connection: non shared network
```

Same output as with iBGP, only difference is that the remote AS and local AS are not the same anymore. And the type changed to external link:

```
BGP neighbor is 172.18.12.1, remote AS 65003, local AS 65002, external link
```

### BGP Router restart methods

Methods to restart BGP peerings are the same for eBGP:

|                                                  |                                                                                                                                                                                                      |
| ------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `execute router restart`                         | <p>Restart entire routing engine (static routing, bgp, ospf, etc)<br>Only during maintenance window</p>                                                                                              |
| `execute router clear bgp ip <neighbor-ip>`      | Restart BGP session to peer with \<neighbor-ip>. Flushes all BGP routes.                                                                                                                             |
| `execute router clear bgp ip <neighbor-ip> soft` | Refreshes routes without tearing down the BGP TCP session. If route refresh is negotiated, FortiGate can request the peer to resend routes; this is useful after changing inbound or outbound policy |

More details: [https://community.fortinet.com/fortigate-3/technical-tip-bgp-soft-reset-to-refresh-bgp-routing-table-without-tearing-down-existing-peering-sessions-92848](https://community.fortinet.com/fortigate-3/technical-tip-bgp-soft-reset-to-refresh-bgp-routing-table-without-tearing-down-existing-peering-sessions-92848)

## Summary

When only looking at the basic peering between two routers, iBGP and eBGP are almost identical. Both use the same BGP state machine, TCP port 179, OPEN messages, keepalives, and capability negotiation.

The main difference in this lab is the AS relationship between the peers. In iBGP, both routers belong to the same Autonomous System. In eBGP, the routers belong to different Autonomous Systems.

The more important differences between iBGP and eBGP become visible later, when routes are advertised and BGP path attributes such as AS path, next-hop, and local preference are introduced.

