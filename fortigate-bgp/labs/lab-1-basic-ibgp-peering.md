# Lab 1: Basic iBGP Peering

## Objective

Just a very basic iBGP peering between two VDOMs. The goal is not to exchange any routes, but to understand what is required for a BGP session to establish and how to verify the session state.

## Topology

<figure><img src="https://github.com/sanderzegers/technotes/raw/refs/heads/undefined/fortigate-bgp/assets/topologies/exports/bgp-lab-master-Lab1.svg" alt=""><figcaption></figcaption></figure>

R1 AS 65001\
R2 AS 65001

## Packet Captures

|                                  |                                                                                                              |
| -------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| lab1-bgp-simple-ibgp-peer.pcapng | Very simple peering, without route announcements. Keep-alive send every 50 secs, with default configuration. |
| lab1-bgp-wrong-as.pcapng         | Remote Router on R1 is configured with wrong remote AS.                                                      |
|                                  |                                                                                                              |

## Peering requirements

To create an ibgp peering between both peers, following settings must match:

| Requirement         | Description                                                                                           |
| ------------------- | ----------------------------------------------------------------------------------------------------- |
| IP reachability     | Each peer must able to reach other peer's BGP address                                                 |
| TCP/179 Allowed     | BGP uses TCP Port 179. If traffic is passing a firewall, traffic must be allowed by a firewall policy |
| Correct Remote AS   | Each router must configure the other router's AS correctly. For iBGP, this is the same  AS            |
| Correct neighbor IP | The configured neighbor address must match the source address used by the peer                        |
| Router ID           | Each BGP router should have a unique router ID                                                        |



## 1st Successful peering

Minimal setup. Define local AS and define a remote bgp peer with the same AS

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

### Verify BGP peering summary

```
FGT02 (R1) # get router info bgp  summary 

VRF 0 BGP router identifier 172.17.0.1, local AS number 65001
BGP table version is 1
0 BGP AS-PATH entries
0 BGP community entries

Neighbor    V         AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
172.18.12.1 4      65001       5       5        0    0    0 00:02:49        0

```

* 172.17.0.1 is the local router id. Since none was defined, highest loopback ip address was used. If no loopback, highest IP address on any configured interface will be used.
* 65001 is the local AS that is configured on the local router
* BGP table version is 1. Revision gets updated every time when local BGP table changes (for example: BGP prefix/path is added, removed, or best-path attributs changes0
* 0 BGP AS-PATH entries
* 0 community entries

#### Neighbor Parameters

|              |                                                                     |
| ------------ | ------------------------------------------------------------------- |
| Neighbor     | IP of remote neighbor                                               |
| V            | BGP Version 4 (Current since 2006)                                  |
| AS           | Remote AS                                                           |
| MsgRcvd      | BGP messages received, during BGP session                           |
| MsgSent      | BGP messages sent, during BGP Session                               |
| TblVer       | Remote BGP Table Version                                            |
| Inq          | Incoming Queue for parsing BGP messages (normally 0)                |
| Outq         | Incoming Queue for parsing BGP messages (normally 0)                |
| Up/Down      | BGP session duration in current state                               |
| State/PfRcxd | Shows state if not established, otherwise number of prefix received |

### Verify BGP Neighbor Status

`get router info bgp neighbors <ip>` shows additional information for the configured neighbor routers:

```
FGT02 (R1) # get router info bgp neighbors 172.18.12.1
VRF 0 neighbor table:
BGP neighbor is 172.18.12.1, remote AS 65001, local AS 65001, internal link
  BGP version 4, remote router ID 172.17.0.2
  BGP state = Established, up for 00:26:17
  Last read 00:00:01, hold time is 180, keepalive interval is 60 seconds
  Configured hold time is 180, keepalive interval is 60 seconds
  Neighbor capabilities:
    Route refresh: advertised and received (old and new)
    Address family IPv4 Unicast: advertised and received
    Address family VPNv4 Unicast: advertised and received
    Address family IPv6 Unicast: advertised and received
    Address family VPNv6 Unicast: advertised and received
    Address family L2VPN EVPN: advertised and received
  Received 34 messages, 0 notifications, 0 in queue
  Sent 32 messages, 0 notifications, 0 in queue
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
Local host: 172.18.12.0, Local port: 179
Foreign host: 172.18.12.1, Foreign port: 6295
Egress interface: 58
Nexthop: 172.18.12.0
Nexthop interface: R1R2-0
Nexthop global: ::
Nexthop local: ::
BGP connection: non shared network
```

|                        |   |
| ---------------------- | - |
| keepalive interval     |   |
|  Neighbor capabilities |   |
| Route refresh request  |   |

### BGP Router restart methods

Methods to restart BGP peerings from most disruptive to the most graceful.

|                                         |                                                                                                                                                              |
| --------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `execute router start`                  | <p>Restart entire routing engine (static routing, bgp, ospf, etc)<br>Only during maintenance window</p>                                                      |
| `execute router clear bgp ip <ip>`      | Restart BGP session to peer with \<ip>. Flushes all BGP routes.                                                                                              |
| `execute router clear bgp ip <ip> soft` | Will use BGP UPDATE message at the next configured advertisement-interval. Can be necessary after changing an outbound route-map, access-list or prefix-list |

More details: [https://community.fortinet.com/fortigate-3/technical-tip-bgp-soft-reset-to-refresh-bgp-routing-table-without-tearing-down-existing-peering-sessions-92848](https://community.fortinet.com/fortigate-3/technical-tip-bgp-soft-reset-to-refresh-bgp-routing-table-without-tearing-down-existing-peering-sessions-92848)



[https://community.fortinet.com/fortigate-3/technical-tip-bgp-router-id-selection-criteria-on-fortigate-193541](https://community.fortinet.com/fortigate-3/technical-tip-bgp-router-id-selection-criteria-on-fortigate-193541)

