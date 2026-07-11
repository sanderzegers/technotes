# Lab 7: BGP path selection

## Objective

In this lab we look at BGP path selection.&#x20;

R1 receives the same prefix from two different eBGP paths. We compare the BGP attributes and see why FortiGate selects one path as the best path and installs it into the routing table.

## Topology

<figure><img src="https://github.com/sanderzegers/technotes/raw/refs/heads/undefined/fortigate-bgp/assets/topologies/exports/bgp-lab-master-Lab6.svg" alt=""><figcaption></figcaption></figure>

<table><thead><tr><th width="111.4166259765625">Router</th><th width="95.8333740234375" align="right">AS</th><th width="182.75">Interface IP</th><th width="201.5">BGP Neighbor</th><th>Local Networks</th></tr></thead><tbody><tr><td>R1</td><td align="right">65001</td><td>R1-R2: <code>172.18.12.0/31</code><br>R1-R3: <code>172.18.13.0/31</code></td><td>R2: <code>172.18.12.1</code><br>R3: <code>172.18.13.1</code></td><td>-</td></tr><tr><td>R2</td><td align="right">65002</td><td>R1-R2: <code>172.18.12.1/31</code><br>R2-R4: <code>172.18.24.0/31</code></td><td>R1: <code>172.18.12.0</code><br>R4: <code>172.18.24.1</code></td><td>-</td></tr><tr><td>R3</td><td align="right">65003</td><td>R1-R3: <code>172.18.13.1/31</code><br>R3-R4: <code>172.18.34.0/31</code></td><td>R1: <code>172.18.13.0</code><br>R4: <code>172.18.34.1</code></td><td>-</td></tr><tr><td>R4</td><td align="right">65004</td><td>R2-R4: <code>172.18.24.1/31</code><br>R3-R4: <code>172.18.34.1/31</code></td><td>R2: <code>172.18.24.0</code><br>R3: <code>172.18.34.0</code></td><td><code>10.10.4.0/24</code></td></tr></tbody></table>

## Packet Captures

<table><thead><tr><th width="174">PCAP File</th><th>Description</th><th>What to look for</th></tr></thead><tbody><tr><td></td><td></td><td></td></tr><tr><td></td><td></td><td></td></tr></tbody></table>

## Path Selection

Let's configure the routers in an eBGP configuration, and R4 announces one single network prefix.

Then we see what path is select on R1:

```
config vdom
    edit R1
        config router bgp
            set as 65001
            set router-id 1.1.1.1

            config neighbor
                edit "172.18.12.1"
                    set remote-as 65002
                next
                edit "172.18.13.1"
                    set remote-as 65003
                next
            end
        end
    next

    edit R2
        config router bgp
            set as 65002
            set router-id 2.2.2.2

            config neighbor
                edit "172.18.12.0"
                    set remote-as 65001
                next
                edit "172.18.24.1"
                    set remote-as 65004
                next
            end
        end
    next

    edit R3
        config router bgp
            set as 65003
            set router-id 3.3.3.3

            config neighbor
                edit "172.18.13.0"
                    set remote-as 65001
                next
                edit "172.18.34.1"
                    set remote-as 65004
                next
            end
        end
    next

    edit R4
        config router bgp
            set as 65004
            set router-id 4.4.4.4

            config neighbor
                edit "172.18.24.0"
                    set remote-as 65002
                next
                edit "172.18.34.0"
                    set remote-as 65003
                next
            end
            config network
                edit 1
                    set prefix 10.10.4.0 255.255.255.0
                next
            end
        end
    next
end
```

Now let's see what we received on R1:

<pre><code>BGP-LAB (R1) # get router info bgp summary 

VRF 0 BGP router identifier 1.1.1.1, local AS number 65001
BGP table version is 1
5 BGP AS-PATH entries
0 BGP community entries

Neighbor    V         AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
172.18.12.1 4      65002       4       4        0    0    0 00:01:12        1
172.18.13.1 4      65003       5       6        1    0    0 00:02:18        1

Total number of neighbors 2


BGP-LAB (R1) # get router info bgp network 
VRF 0 BGP table version is 1, local router ID is 1.1.1.1
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              S Stale
Origin codes: i - IGP, e - EGP, ? - incomplete

   Network          Next Hop            Metric     LocPrf Weight RouteTag Path
*  10.10.4.0/24     172.18.13.1     0                      0        0 65003 65004 i &#x3C;-/->
<strong>*>                  172.18.12.1     0                      0        0 65002 65004 i &#x3C;-/1>
</strong>
Total number of prefixes 1

BGP-LAB (R1) # get router info routing-table bgp
Routing table for VRF=0
B       10.10.4.0/24 [20/0] via 172.18.12.1 (recursive is directly connected, R1R2-0), 00:01:19, [1/0]

BGP-LAB (R1) # get router info bgp network 10.10.4.0/24
VRF 0 BGP routing table entry for 10.10.4.0/24
Paths: (2 available, best #2, table Default-IP-Routing-Table)
  Advertised to non peer-group peers:
   172.18.13.1
  Original VRF 0
  65003 65004
    172.18.13.1 from 172.18.13.1 (3.3.3.3)
      Origin IGP distance 20 metric 0, localpref 100, valid, external
      Last update: Tue Jul  7 14:29:38 2026

  Original VRF 0
  65002 65004
    172.18.12.1 from 172.18.12.1 (2.2.2.2)
<strong>      Origin IGP distance 20 metric 0, localpref 100, valid, external, best
</strong>      Last update: Tue Jul  7 14:29:25 2026


</code></pre>

R1 receives two BGP paths to `10.10.4.0/24`.

One path is learned via R2, and the other path is learned via R3. Both paths have a different next-hop address and a different AS path. The AS paths are different, but the AS path length is equal. Both paths have an AS path length of 2, so AS path length does not decide the winner in this example.

However, only one of these paths is selected as the best path and installed in the local routing table, also called the RIB.

Why?

BGP does not install all received paths into the routing table by default. Instead, it uses the BGP best path selection algorithm to choose the preferred path.

Unfortunately, FortiGate does not show a direct explanation such as "this route was selected because of local preference" or "this route was selected because of the lower router ID". It only shows which path is selected as best.

To understand why a route was chosen, we need to compare the BGP attributes of both paths and exclude the attributes that are equal.

Let’s compare the attributes for `10.10.4.0/24` via R2 and R3.

| Attribute                                                     |                                     Via R3 |                                                    Via R2 | Winner                                  |
| ------------------------------------------------------------- | -----------------------------------------: | --------------------------------------------------------: | --------------------------------------- |
| Weight                                                        |                                          0 |                                                         0 | Equal                                   |
| Local preference                                              |                                        100 |                                                       100 | Equal                                   |
| Route originated by local router                              |                                         No |                                                        No | Equal                                   |
| AS path length                                                |                                          2 |                                                         2 | Equal                                   |
| Origin                                                        |                                        IGP |                                                       IGP | Equal                                   |
| MED / metric                                                  |                                          0 |                                                         0 | Equal                                   |
| Route type                                                    |                                   external |                                                  external | Equal                                   |
| IGP metric to next-hop                                        |                         directly connected |                                        directly connected | Equal                                   |
| <mark style="color:$success;">Prefer oldest eBGP route</mark> | <mark style="color:$success;">newer</mark> | <mark style="color:$success;">older / current best</mark> | <mark style="color:$success;">R2</mark> |
| Prefer lowest neighbor IP address                             |                              `172.18.13.1` |                                             `172.18.12.1` | R2                                      |

## Summary

At this point we only observed the BGP best-path decision. In the next lab we will influence the decision ourselves by using route-maps to change BGP attributes such as local preference.



## Links

{% embed url="https://community.fortinet.com/fortigate-3/technical-tip-bgp-route-selection-process-97731" %}
