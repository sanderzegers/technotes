# Lab 11: BGP Multipath and Equal-Cost Load Balancing

## Objective

In this lab, you will enable BGP multipath on R1 so that FortiGate can install two eligible eBGP paths for the same prefix. You will verify ECMP forwarding, make the paths unequal, and test how FortiGate reacts to a path failure.

{% hint style="info" %}
BGP multipath does not replace the best-path algorithm. FortiGate still compares the paths first. Multipath allows multiple sufficiently equal paths to be installed instead of discarding all but one after the relevant comparisons tie.
{% endhint %}

## Topology

This lab reuses the topology from Labs 7 through 10. R4 advertises the same prefixes through R2 and R3, which remain in different transit ASes. R1 therefore receives two eBGP paths with different next hops and different `AS_PATH` values. The AS paths have the same length, and the other relevant path-selection attributes are equal.

<figure><img src="https://github.com/sanderzegers/technotes/raw/refs/heads/undefined/fortigate-bgp/assets/topologies/exports/bgp-lab-master-Lab6.svg" alt=""><figcaption></figcaption></figure>

| Router |    AS | Router ID | Interfaces                                                                      | BGP neighbors                                                       | Local networks                                                 |
| ------ | ----: | --------- | ------------------------------------------------------------------------------- | ------------------------------------------------------------------- | -------------------------------------------------------------- |
| R1     | 65001 | `1.1.1.1` | <p>R1-R2: <code>172.18.12.0/31</code><br>R1-R3: <code>172.18.13.0/31</code></p> | <p>R2: <code>172.18.12.1</code><br>R3: <code>172.18.13.1</code></p> | <p><code>10.10.1.0/24</code><br><code>172.17.0.1/32</code></p> |
| R2     | 65002 | `2.2.2.2` | <p>R1-R2: <code>172.18.12.1/31</code><br>R2-R4: <code>172.18.24.0/31</code></p> | <p>R1: <code>172.18.12.0</code><br>R4: <code>172.18.24.1</code></p> | -                                                              |
| R3     | 65003 | `3.3.3.3` | <p>R1-R3: <code>172.18.13.1/31</code><br>R3-R4: <code>172.18.34.0/31</code></p> | <p>R1: <code>172.18.13.0</code><br>R4: <code>172.18.34.1</code></p> | -                                                              |
| R4     | 65004 | `4.4.4.4` | <p>R2-R4: <code>172.18.24.1/31</code><br>R3-R4: <code>172.18.34.1/31</code></p> | <p>R2: <code>172.18.24.0</code><br>R3: <code>172.18.34.0</code></p> | <p><code>10.10.4.0/24</code><br><code>172.17.0.4/32</code></p> |

## BGP Multipath and ECMP

BGP multipath and ECMP describe related but different parts of the process.

BGP multipath adds multiple valid BGP paths to the routing table. ECMP then spreads traffic across them.

On FortiGate, eBGP and iBGP multipath are controlled separately:

```
config router bgp
    set ebgp-multipath enable
    set ibgp-multipath enable
end
```

Only `ebgp-multipath` is used in this lab.

The number of ECMP next hops and the forwarding algorithm are configured separately in the VDOM system settings. These are the default settings:

```
config system settings
    set ecmp-max-paths 255
    set v4-ecmp-mode source-ip-based
end
```

## Baseline Configuration

Restore the lab baseline and configure the four-router eBGP topology.

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
            config network
                edit 1
                    set prefix 10.10.1.0 255.255.255.0
                next
                edit 2
                    set prefix 172.17.0.1 255.255.255.255
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
                edit 2
                    set prefix 172.17.0.4 255.255.255.255
                next
            end
        end
    next
end
```

Verify that all BGP sessions are established:

```
BGP-LAB (R1) # get router info bgp summary

VRF 0 BGP router identifier 1.1.1.1, local AS number 65001
BGP table version is 2
5 BGP AS-PATH entries
0 BGP community entries

Neighbor    V         AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
172.18.12.1 4      65002       4       5        1    0    0 00:01:09        2
172.18.13.1 4      65003       4       8        0    0    0 00:00:04        2

Total number of neighbors 2

BGP-LAB (R1) # sudo R4 get router info bgp summary

VRF 0 BGP router identifier 4.4.4.4, local AS number 65004
BGP table version is 2
7 BGP AS-PATH entries
0 BGP community entries

Neighbor    V         AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
172.18.24.0 4      65002       4       6        1    0    0 00:01:36        2
172.18.34.0 4      65003       7      17        2    0    0 00:00:33        2

Total number of neighbors 2
```

R1 should receive both R4 prefixes from R2 and R3. R4 should receive both R1 prefixes through the same two transit routers.

## Exercise 1: Observe the Default Single Path

Multipath is disabled by default. Inspect R1's BGP table:

<pre><code>BGP-LAB (R1) # get router info bgp network 10.10.4.0/24
VRF 0 BGP routing table entry for 10.10.4.0/24
Paths: (2 available, best #2, table Default-IP-Routing-Table)
  Advertised to non peer-group peers:
   172.18.13.1
  Original VRF 0
  65003 65004
    172.18.13.1 from 172.18.13.1 (3.3.3.3)
      Origin IGP distance 20 metric 0, localpref 100, valid, external
      Last update: Thu Jul 16 12:04:40 2026

  Original VRF 0
  65002 65004
<strong>    172.18.12.1 from 172.18.12.1 (2.2.2.2)
</strong><strong>      Origin IGP distance 20 metric 0, localpref 100, valid, external, best
</strong>      Last update: Thu Jul 16 12:03:31 2026

</code></pre>

R1 has two valid paths:

| Path   | Next hop      | AS path       |
| ------ | ------------- | ------------- |
| Via R2 | `172.18.12.1` | `65002 65004` |
| Via R3 | `172.18.13.1` | `65003 65004` |

The weight, local preference, AS-path length, origin, MED, route type, and cost to the next hops are equal.\
The AS\_PATH values differ, but both contain two AS numbers. BGP therefore continues through its later tie breakers and selects one path as best.

In this capture, the path through R2 is selected. Because that route is older, it wins before the router-ID and neighbor-address tie breakers are considered. \
Your FortiGate may select the other path if the sessions were established in a different order.

Now inspect the routing table:

<pre><code>BGP-LAB (R1) # get router info routing-table bgp
Routing table for VRF=0
<strong>B       10.10.4.0/24 [20/0] via 172.18.12.1 (recursive is directly connected, R1R2-0), 00:02:46, [1/0]
</strong>B       172.17.0.4/32 [20/0] via 172.18.12.1 (recursive is directly connected, R1R2-0), 00:02:46, [1/0]

</code></pre>

Only one next hop for `10.10.4.0/24` should be installed. Having two paths in the BGP table does not automatically produce ECMP in the routing table.

## Exercise 2: Enable eBGP Multipath

Enable eBGP multipath on R1:

```
config vdom
    edit R1
        config router bgp
            set ebgp-multipath enable
        end
    next
end
```

FortiGate should re-evaluate the existing paths. If the routing table does not update immediately, request a soft inbound refresh:

```
sudo R1 execute router clear bgp all soft in
```

Verify the BGP and routing tables:

<pre><code>BGP-LAB (R1) # get router info bgp network 10.10.4.0/24
VRF 0 BGP routing table entry for 10.10.4.0/24
Paths: (2 available, best #2, table Default-IP-Routing-Table)
  Advertised to non peer-group peers:
   172.18.13.1
  Original VRF 0
  65003 65004
    172.18.13.1 from 172.18.13.1 (3.3.3.3)
<strong>      Origin IGP distance 20 metric 0, localpref 100, valid, external
</strong>      Last update: Thu Jul 16 12:07:21 2026

  Original VRF 0
  65002 65004
    172.18.12.1 from 172.18.12.1 (2.2.2.2)
<strong>      Origin IGP distance 20 metric 0, localpref 100, valid, external, best
</strong>      Last update: Thu Jul 16 12:07:03 2026


BGP-LAB (R1) # get router info routing-table bgp
Routing table for VRF=0
<strong>B       10.10.4.0/24 [20/0] via 172.18.12.1 (recursive is directly connected, R1R2-0), 00:00:08, [1/0]
</strong><strong>                     [20/0] via 172.18.13.1 (recursive is directly connected, R1R3-1-0), 00:00:08, [1/0]
</strong>B       172.17.0.4/32 [20/0] via 172.18.12.1 (recursive is directly connected, R1R2-0), 00:00:08, [1/0]
                      [20/0] via 172.18.13.1 (recursive is directly connected, R1R3-1-0), 00:00:08, [1/0]

</code></pre>

The routing table should now contain both next hops for the same prefix.

{% hint style="info" %}
One path may still be labeled `best` in detailed BGP output. Multipath does not remove the concept of a best path. It allows additional equal paths to join the best path in the routing table.
{% endhint %}

## Exercise 3: Make the Paths Unequal

Multipath requires eligible paths. In this exercise, apply a higher local preference to the route received from R3.

Create a prefix-list and inbound route-map on R1:

<pre><code>config vdom
    edit R1
        config router prefix-list
            edit "PL_R4_LAN"
                config rule
                    edit 10
                        set prefix 10.10.4.0 255.255.255.0
                    next
                end
            next
        end
        config router route-map
            edit "RM_PREF_R3_IN"
                config rule
<strong>                    edit 10
</strong><strong>                        set match-ip-address "PL_R4_LAN"
</strong><strong>                        set set-local-preference 200
</strong>                    next
<strong>                    edit 20
</strong><strong>                    next
</strong>                end
            next
        end
        config router bgp
            config neighbor
                edit "172.18.13.1"
                    set route-map-in "RM_PREF_R3_IN"
                next
            end
        end
    next
end
</code></pre>

Rule 20 permits unmatched routes without changing them. This prevents the route-map's implicit deny from filtering other prefixes.

Refresh the routes received from R3:

```
sudo R1 execute router clear bgp ip 172.18.13.1 soft in
```

Verify the result:

<pre><code>BGP-LAB (R1) # get router info bgp network 10.10.4.0/24
VRF 0 BGP routing table entry for 10.10.4.0/24
Paths: (2 available, best #1, table Default-IP-Routing-Table)
  Advertised to non peer-group peers:
   172.18.12.1
  Original VRF 0
  65003 65004
    172.18.13.1 from 172.18.13.1 (3.3.3.3)
<strong>      Origin IGP distance 20 metric 0, localpref 200, valid, external, best
</strong>      Last update: Thu Jul 16 12:09:41 2026

  Original VRF 0
  65002 65004
    172.18.12.1 from 172.18.12.1 (2.2.2.2)
<strong>      Origin IGP distance 20 metric 0, localpref 100, valid, external
</strong>      Last update: Thu Jul 16 12:07:03 2026



BGP-LAB (R1) # get router info routing-table bgp
Routing table for VRF=0
<strong>B       10.10.4.0/24 [20/0] via 172.18.13.1 (recursive is directly connected, R1R3-1-0), 00:00:55, [1/0]
</strong>B       172.17.0.4/32 [20/0] via 172.18.12.1 (recursive is directly connected, R1R2-0), 00:03:00, [1/0]
                      [20/0] via 172.18.13.1 (recursive is directly connected, R1R3-1-0), 00:03:00, [1/0]


</code></pre>

The R3 path should have local preference 200, while the R2 path keeps the default value of 100. Both paths can remain visible in the BGP table, but only the higher-local-preference path through R3 should be installed in the routing table.

This demonstrates the central multipath rule: enabling `ebgp-multipath` does not make unequal paths equal.

Restore equal attributes:

```
config vdom
    edit R1
        config router bgp
            config neighbor
                edit "172.18.13.1"
                    unset route-map-in
                next
            end
        end
    next
end
```

Refresh the neighbor again and confirm that both next hops return:

```
BGP-LAB (R1) # execute router clear bgp ip 172.18.13.1 soft in

BGP-LAB (R1) # get router info routing-table bgp
Routing table for VRF=0
B       10.10.4.0/24 [20/0] via 172.18.13.1 (recursive is directly connected, R1R3-1-0), 00:00:55, [1/0]
                     [20/0] via 172.18.12.1 (recursive is directly connected, R1R2-0), 00:00:55, [1/0]
B       172.17.0.4/32 [20/0] via 172.18.12.1 (recursive is directly connected, R1R2-0), 00:05:07, [1/0]
                      [20/0] via 172.18.13.1 (recursive is directly connected, R1R3-1-0), 00:05:07, [1/0]


```

## Exercise 4: Test ECMP Forwarding

With both next hops installed, FortiGate selects an ECMP member for each new session. The default mode used in this lab is `source-ip-based`.

{% hint style="warning" %}
If SD-WAN is enabled, the ECMP load-balancing mode is configured under `config system sdwan` instead of with `v4-ecmp-mode`.&#x20;
{% endhint %}

| ECMP mode              | Behavior                                                                                                           |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------ |
| `source-ip-based`      | Sessions with the same source IP use the same ECMP path                                                            |
| `source-dest-ip-based` | The source and destination IP addresses are used to select a path                                                  |
| `weight-based`         | Sessions are distributed according tot he configured ECMP weights. This is separate from the BGP weight attribute. |
| `usage-based`          | Traffic spills over to another path when the configured bandwidth threshold is reached.                            |

|   |
| - |

| Sessions are distributed according to the configured ECMP weights. This is separate from the BGP weight attribute. |
| ------------------------------------------------------------------------------------------------------------------ |

|   |
| - |

Verify the current settings on R1:

```
BGP-LAB (R1) # show full system settings | grep ecmp
    set v4-ecmp-mode source-ip-based
    set ecmp-max-paths 255
```

Because ECMP selection is session-based, repeatedly sending the same flow does not prove load balancing. A single source and destination normally remain on the same path. Generate traffic from multiple source IP addresses or from multiple clients behind R1.

To make the forwarding test (traceroute) work in both directions, enable eBGP multipath on R4 so that both return paths toward R1 are installed.

Without `ebgp-multipath` on R4, only one route back toward R1 is active. If a probe arrives through the other transit router, R4’s default feasible-path RPF check may drop it because no active route to the source uses the incoming interface. Enabling multipath installs both return paths, so probes arriving through either R2 or R3 can pass the check and receive replies.

```
config vdom
    edit R4
        config router bgp
           set ebgp-multipath enable
        end
   next
end
```

```
BGP-LAB (R4) # execute router clear bgp all soft in
```

From R1 run a traceroute with two different source IP addresses:

<pre><code>BGP-LAB (R1) # execute traceroute-options source 10.10.1.1

BGP-LAB (R1) # execute traceroute 10.10.4.1
traceroute to 10.10.4.1 (10.10.4.1), 32 hops max, 1 probe packets per hop, 72 byte packets
<strong> 1  172.18.12.1  0.348 ms
</strong> 2  10.10.4.1  0.453 ms

BGP-LAB (R1) # execute traceroute-options source 172.17.0.1

BGP-LAB (R1) # execute traceroute 10.10.4.1
traceroute to 10.10.4.1 (10.10.4.1), 32 hops max, 1 probe packets per hop, 72 byte packets
<strong> 1  172.18.13.1  0.330 ms
</strong> 2  10.10.4.1  0.492 ms

</code></pre>

The hash can place different flows on different paths, but two test sources are not guaranteed to exercise both ECMP members. For a stronger test, use several client source addresses.

## Exercise 5: Test Path Failure

Disable the link from R1 to R2:

```
config vdom
    edit R1
        config system interface
            edit "R1R2-0"
                set status down
            next
        end
    next
end
```

Verify the BGP session and route from R1:

```
BGP-LAB (R1) # get router info bgp summary 

VRF 0 BGP router identifier 1.1.1.1, local AS number 65001
BGP table version is 9
6 BGP AS-PATH entries
0 BGP community entries

Neighbor    V         AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
172.18.12.1 4      65002      84      88        0    0    0    never Active     
172.18.13.1 4      65003      85      91        5    0    0 01:04:33        2

Total number of neighbors 2


BGP-LAB (R1) # get router info routing-table bgp
Routing table for VRF=0
B       10.10.4.0/24 [20/0] via 172.18.13.1 (recursive is directly connected, R1R3-1-0), 00:00:13, [1/0]
B       172.17.0.4/32 [20/0] via 172.18.13.1 (recursive is directly connected, R1R3-1-0), 00:00:13, [1/0]


BGP-LAB (R1) # execute ping 10.10.4.1
PING 10.10.4.1 (10.10.4.1): 56 data bytes
64 bytes from 10.10.4.1: icmp_seq=0 ttl=254 time=0.5 ms
64 bytes from 10.10.4.1: icmp_seq=1 ttl=254 time=0.2 ms
^C
--- 10.10.4.1 ping statistics ---
2 packets transmitted, 2 packets received, 0% packet loss
round-trip min/avg/max = 0.2/0.3/0.5 ms
```

The R2 path should disappear. The route through R3 should remain installed, and traffic should continue over the surviving path after convergence.

Re-enable the link:

```
config vdom
    edit R1
        config system interface
            edit "R1R2-0"
                set status up
            next
        end
    next
end
```

After the BGP session is re-established and the route is received again, both next hops should return to the routing table.

## Summary

In this lab, R1 received two equal eBGP paths to prefixes advertised by R4. By default, both paths appeared in the BGP table, but only one was installed in the routing table.

After enabling `ebgp-multipath`, R1 installed both eligible next hops and used them for ECMP forwarding. You then changed the local preference on one path and confirmed that unequal paths no longer formed an ECMP route.

You also reviewed the per-VDOM `ecmp-max-paths` and `v4-ecmp-mode` settings, tested forwarding with multiple flows, and verified that traffic continued over the remaining path after a link failure.

BGP multipath extends the best-path process; it does not replace it. The paths must first be valid and sufficiently equal before FortiGate can install them together.

## Links

{% embed url="https://docs.fortinet.com/document/fortigate/7.6.0/administration-guide/25967/equal-cost-multi-path" %}
