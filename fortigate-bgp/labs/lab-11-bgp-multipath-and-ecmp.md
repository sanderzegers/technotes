# Lab 11: BGP Multipath and Equal-Cost Load Balancing

## Objective

In Lab 7, R1 received two valid paths for the same prefix, but BGP selected only one best path for installation in the routing table. In this lab, you will enable BGP multipath so that FortiGate can install both eligible eBGP paths and use them for equal-cost multipath forwarding (ECMP).

You will:

* observe the default single-path behavior;
* enable eBGP multipath on R1;
* verify that two BGP next hops are installed in the routing table;
* make the paths unequal and observe ECMP being removed;
* review the FortiGate ECMP forwarding algorithm;
* test forwarding and path failure.

{% hint style="info" %}
BGP multipath does not replace the best-path algorithm. FortiGate still compares the paths first. Multipath allows multiple sufficiently equal paths to be installed instead of discarding all but one after the relevant comparisons tie.
{% endhint %}

## Topology

This lab reuses the diamond topology from Labs 7 through 10. R2 and R3 are placed in the same transit AS so that the paths received by R1 have identical AS paths.

<figure><img src="https://github.com/sanderzegers/technotes/raw/refs/heads/undefined/fortigate-bgp/assets/topologies/exports/bgp-lab-master-Lab6.svg" alt=""><figcaption></figcaption></figure>

```text
                  R2 (AS 65002)
                 /             \
                /               \
       R1 (AS 65001)        R4 (AS 65004)
                \               /
                 \             /
                  R3 (AS 65002)
```

| Router | AS | Router ID | Interfaces | BGP neighbors | Local networks |
| ------ | --: | --------- | ---------- | ------------- | -------------- |
| R1 | 65001 | `1.1.1.1` | R1-R2: `172.18.12.0/31`<br>R1-R3: `172.18.13.0/31` | R2: `172.18.12.1`<br>R3: `172.18.13.1` | `10.10.1.0/24`<br>`172.17.0.1/32` |
| R2 | 65002 | `2.2.2.2` | R1-R2: `172.18.12.1/31`<br>R2-R4: `172.18.24.0/31` | R1: `172.18.12.0`<br>R4: `172.18.24.1` | - |
| R3 | 65002 | `3.3.3.3` | R1-R3: `172.18.13.1/31`<br>R3-R4: `172.18.34.0/31` | R1: `172.18.13.0`<br>R4: `172.18.34.1` | - |
| R4 | 65004 | `4.4.4.4` | R2-R4: `172.18.24.1/31`<br>R3-R4: `172.18.34.1/31` | R2: `172.18.24.0`<br>R3: `172.18.34.0` | `10.10.4.0/24`<br>`172.17.0.4/32` |

R4 advertises the same prefixes through R2 and R3. R1 therefore receives two paths with the same BGP attributes but different next hops.

## Packet Captures

No packet capture is included for this lab yet.

BGP UPDATE messages do not contain a special ECMP instruction. R1 independently decides whether the received paths are eligible for multipath installation. A packet capture can show the two equivalent advertisements, while the FortiGate BGP and routing tables show the local multipath decision.

## BGP Multipath and ECMP

BGP multipath and ECMP describe related but different parts of the process:

| Function | Purpose |
| -------- | ------- |
| BGP multipath | Allows more than one eligible BGP path for a prefix to be installed in the routing table |
| ECMP | Selects a next hop for new traffic when the routing table contains multiple equal-cost next hops |

On FortiGate, eBGP and iBGP multipath are controlled separately:

```
config router bgp
    set ebgp-multipath enable
    set ibgp-multipath enable
end
```

Only `ebgp-multipath` is used in this lab.

The number of ECMP next hops and the forwarding algorithm are configured separately in the VDOM system settings:

```
config system settings
    set ecmp-max-paths 2
    set v4-ecmp-mode source-ip-based
end
```

{% hint style="warning" %}
`maximum-paths` is a command used on some other BGP implementations. It is not the FortiGate BGP command used in this lab. On FortiGate, enable `ebgp-multipath` or `ibgp-multipath`, and use `ecmp-max-paths` under `config system settings` to limit the number of installed ECMP paths.
{% endhint %}

## Baseline Configuration

Restore the lab baseline and configure the four-router eBGP topology.

```
config vdom
    edit R1
        config system settings
            set ecmp-max-paths 2
            set v4-ecmp-mode source-ip-based
        end
        config router bgp
            set as 65001
            set router-id 1.1.1.1
            config neighbor
                edit "172.18.12.1"
                    set remote-as 65002
                next
                edit "172.18.13.1"
                    set remote-as 65002
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
            set as 65002
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
                    set remote-as 65002
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
sudo R1 get router info bgp summary
sudo R4 get router info bgp summary
```

R1 should receive both R4 prefixes from R2 and R3. R4 should receive both R1 prefixes through the same two transit routers.

## Exercise 1: Observe the Default Single Path

Multipath is disabled by default. Inspect R1's BGP table:

```
sudo R1 get router info bgp network 10.10.4.0/24
```

R1 should have two valid paths:

| Path | Next hop | AS path |
| ---- | -------- | ------- |
| Via R2 | `172.18.12.1` | `65002 65004` |
| Via R3 | `172.18.13.1` | `65002 65004` |

The weight, local preference, AS path, origin, MED, route type, and next-hop reachability are equal. BGP continues through its tie breakers and selects one path as best.

Now inspect the routing table:

```
sudo R1 get router info routing-table bgp
```

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

```
sudo R1 get router info bgp network 10.10.4.0/24
sudo R1 get router info routing-table bgp
```

The routing table should now contain both next hops for the same prefix:

```text
B       10.10.4.0/24 [20/0] via 172.18.12.1, R1R2-0
                               via 172.18.13.1, R1R3-1-0
```

The exact formatting and path order can differ between FortiOS releases. The important result is that both next hops are listed under one BGP route.

{% hint style="info" %}
One path may still be labeled `best` in detailed BGP output. Multipath does not remove the concept of a best path. It allows additional equal paths to join the best path in the routing table.
{% endhint %}

## Exercise 3: Make the Paths Unequal

Multipath requires eligible paths. In this exercise, apply a higher local preference to the route received from R3.

Create a prefix-list and inbound route-map on R1:

```
config vdom
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
                    edit 10
                        set match-ip-address "PL_R4_LAN"
                        set set-local-preference 200
                    next
                    edit 20
                    next
                end
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
```

Rule 20 permits unmatched routes without changing them. This prevents the route-map's implicit deny from filtering other prefixes.

Refresh the routes received from R3:

```
sudo R1 execute router clear bgp ip 172.18.13.1 soft in
```

Verify the result:

```
sudo R1 get router info bgp network 10.10.4.0/24
sudo R1 get router info routing-table bgp
```

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
sudo R1 execute router clear bgp ip 172.18.13.1 soft in
sudo R1 get router info routing-table bgp
```

## Exercise 4: Understand ECMP Forwarding

With both next hops installed, FortiGate selects an ECMP member for each new session. The default mode used in this lab is `source-ip-based`.

| ECMP mode | Behavior |
| --------- | -------- |
| `source-ip-based` | Sessions with the same source IP use the same ECMP path |
| `source-dest-ip-based` | The source and destination IP addresses are used to select a path |
| `weight-based` | Sessions are distributed according to configured route weights |
| `usage-based` | A path is used until its configured bandwidth threshold is reached |

Verify the current settings on R1:

```
sudo R1 show full-configuration system settings
```

Look for:

```text
set ecmp-max-paths 2
set v4-ecmp-mode source-ip-based
```

Because ECMP selection is session-based, repeatedly sending the same flow does not prove load balancing. A single source and destination normally remain on the same path. Generate traffic from multiple source IP addresses or from multiple clients behind R1.

For a simple lab test, start a sniffer on R1:

```
sudo R1 diagnose sniffer packet any 'icmp and host 10.10.4.1' 4 0 l
```

From another administrator session, generate test traffic with different sources:

```
sudo R1 execute ping-options repeat-count 4
sudo R1 execute ping-options source 10.10.1.1
sudo R1 execute ping 10.10.4.1

sudo R1 execute ping-options source 172.17.0.1
sudo R1 execute ping 10.10.4.1
```

The hash can place different flows on different paths, but two test sources are not guaranteed to exercise both ECMP members. For a stronger test, use several client source addresses and inspect whether packets leave through both `R1R2-0` and `R1R3-1-0`.

Stop the sniffer with `Ctrl+C` and reset the ping source when finished:

```
sudo R1 execute ping-options reset
```

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

Verify the BGP session and route:

```
sudo R1 get router info bgp summary
sudo R1 get router info routing-table bgp
sudo R1 execute ping 10.10.4.1
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

## BGP Multipath Is Not BGP ADD-PATH

This lab receives the prefix from two different neighbors, so R1 already knows both paths.

`ebgp-multipath` controls whether multiple eligible paths can be installed locally. BGP ADD-PATH is a separate capability that allows a BGP speaker to advertise more than one path for the same prefix to the same neighbor. ADD-PATH becomes relevant in designs where an intermediate router or route reflector would otherwise advertise only its single best path.

## Summary

In this lab, R1 received two equivalent eBGP paths to prefixes originated by R4. With the default configuration, both paths appeared in the BGP table, but only one best path was installed in the routing table.

After enabling `ebgp-multipath`, R1 installed both eligible next hops and could use them for ECMP forwarding. You then changed the local preference on one path and confirmed that unequal paths no longer formed an ECMP route.

You also reviewed the per-VDOM `ecmp-max-paths` and `v4-ecmp-mode` settings, tested forwarding with multiple flows, and verified that traffic continued over the remaining route when one link failed.

The main lesson is that BGP multipath is an extension of best-path processing, not a replacement for it. Multiple paths must first be valid and sufficiently equal before FortiGate can install them together.

## Links

{% embed url="https://docs.fortinet.com/document/fortigate/7.6.0/administration-guide/25967/equal-cost-multi-path" %}

{% embed url="https://docs.fortinet.com/document/fortigate/7.4.0/cli-reference/528620/config-router-bgp" %}

{% embed url="https://community.fortinet.com/fortigate-3/technical-tip-usage-of-bgp-multipath-and-description-of-the-bgp-nlri-table-97722" %}

