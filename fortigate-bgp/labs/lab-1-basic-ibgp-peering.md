# Lab 1: Basic iBGP Peering

## Objective

Just a very basic iBGP peering between two VDOMs. The goal is not to exchange many routes, but to understand what is required for a BGP session to establish and how to verify the session state.

## Topology

<figure><img src="https://github.com/sanderzegers/technotes/raw/refs/heads/undefined/fortigate-bgp/assets/topologies/exports/bgp-lab-master-Lab1.svg" alt=""><figcaption></figcaption></figure>

R1 AS 65001\
R2 AS 65001



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

Verify BGP peering summary

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



Neighbor

IP: 172.18.12.1.4\
V: Version\
AS: Remote AS\
MsgRcsd: BGP messages received\
MsgSent: BGP messages send\
TblVer: Remote Table Version\
Inq: Incoming Queue for parsing BGP messages (normally 0)\
Outq: BGP messages in Outcoming Queue (normally 0)\
Up/Down: BGP session duration in current state\
State/PfRcxd: Shows state if not established, otherwise number of prefix received



[https://community.fortinet.com/fortigate-3/technical-tip-bgp-router-id-selection-criteria-on-fortigate-193541](https://community.fortinet.com/fortigate-3/technical-tip-bgp-router-id-selection-criteria-on-fortigate-193541)
