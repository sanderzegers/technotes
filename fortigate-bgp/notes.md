# Notes

External BGP: Between AS\
Internal BGP: Within AS

Range 1 – 64511 are globally unique AS numbers\
Range 64512 – 65535 are private autonomous system numbers.

BGP is a path vector routing protocol

It stores the path we have to go through to reach the destination network

Just a small note on timers: the timers configured on the FortiGate are not exact delays. RFC 4271 explicitly allows jitter to be applied to BGP timers to prevent peers from sending updates in synchronized bursts. The RFC suggests using a random factor between 0.75 and 1.0 of the configured timer value.



## BGP introductionary courses and sources

Books:

Internet Routing Architectures (Second Edition) Sam Halabi

Online Courses:

[https://networklessons.com/bgp](https://networklessons.com/bgp)

eBGP simulation:

{% embed url="https://nsg.ee.ethz.ch/blog/2020-04-14_mini_internet/" %}

{% embed url="https://github.com/nsg-ethz/mini_internet_project?utm_source=chatgpt.com" %}

{% embed url="https://dn42.dev/services/Route-Collector?utm_source=chatgpt.com" %}

## Lab draft

• Lab 11: Loopback Peering and update-source

Summary: This lab introduces BGP peering over loopback interfaces instead of directly connected interface addresses. It shows why loopback-based peering is commonly used in larger or more stable designs, and why underlying reachability plus update-source are required for the session to establish.

Tasks:

* Create loopback interfaces on two or more routers and assign stable BGP peering IPs.
* Verify that loopback addresses are not reachable yet from the remote peer.
* Add static routes or underlay routing so the loopback addresses become reachable.
* Configure BGP neighbors to use loopback IPs instead of interface IPs.
* Apply update-source so the session uses the loopback as the source address.
* Verify session establishment with get router info bgp summary and get router info bgp neighbors.
* Break underlay reachability and observe the effect on the BGP session.
* Restore reachability and confirm session recovery.

Lab 12: AS-Path Prepending

Summary: This lab shows how to influence inbound path selection from a remote AS by prepending the local AS multiple times on advertised routes. The reader verifies how the AS path changes in BGP updates and how a remote router prefers the shorter path when multiple routes to the same prefix are available.

Tasks:

* Build a topology where one prefix can be learned through two external paths.
* Verify the initial best path on the remote router before any policy is applied.
* Create a route-map that prepends the local AS on one advertised route.
* Apply the route-map outbound to the chosen neighbor.
* Perform a soft outbound reset so the new policy is advertised.
* Verify on the receiving router that the AS path is now longer on one path.
* Confirm that the remote router changes or keeps its best path based on AS-path length.
* Remove the prepending policy and verify that the original behavior returns.

Lab 13: MED

Summary: This lab introduces the Multi-Exit Discriminator attribute and shows how it can be used to suggest a preferred entry point into an AS when multiple links exist between the same neighboring autonomous systems. The lab focuses on how MED is advertised, received, and compared in the best-path process.

Tasks:

* Create a topology with two links between the same neighboring ASes.
* Advertise the same prefix across both paths.
* Verify the initial best path selection on the receiving router.
* Create a route-map to set a lower MED on one path and a higher MED on the other.
* Apply the route-map outbound on the advertising side.
* Perform a soft reset and verify that the MED values are present in the received paths.
* Confirm that the lower MED path is preferred when other attributes are equal.
* Change the MED values and observe how best-path selection changes.

Lab 14: Multipath / ECMP

Summary: This lab shows that BGP does not always have to install only one path. When multiple paths are considered equal and multipath is enabled, FortiGate can install more than one route into the routing table. This lab helps the reader compare ordinary best-path behavior with equal-cost multipath behavior.

Tasks:

* Build a topology where the same prefix is learned through two equivalent paths.
* Verify that only one best path is installed by default.
* Compare the received BGP attributes and confirm that both paths are otherwise eligible.
* Enable BGP multipath on the receiving router.
* Refresh the BGP session or routes as required.
* Verify that both paths are installed in the routing table.
* Test forwarding behavior across both next hops if supported by the lab.
* Disable multipath again and confirm that the routing table returns to a single best path.

Lab 15: Troubleshooting and Route Refresh

Summary: This lab turns operational troubleshooting into a structured exercise. It combines neighbor verification, advertised and received route inspection, policy checks, soft resets, and route refresh behavior so the reader learns how to diagnose why a route is missing, rejected, or not preferred.

Tasks:

* Start from a working BGP scenario with known expected routes.
* Introduce a fault such as a wrong remote AS, missing network statement, unreachable next hop, or filtering policy.
* Use get router info bgp summary to identify session state problems.
* Use get router info bgp neighbors, routes, and advertised-routes to locate where the route is lost.
* Inspect prefix-lists, community-lists, and route-maps applied to the neighbor.
* Correct the fault and use soft reset or route refresh where appropriate.
* Compare soft inbound, soft outbound, and hard reset behavior.
* Verify that the route reappears and that the final best path matches expectations.

If you want, I can turn these into GitBook-ready chapter skeletons with Objective, Topology, Tasks, and Expected outcome sections.

### Access-lists

A simple list based on a prefix consisting of an IPv4 or IPv6 address and netmask or cisco wildcard mask.

Simple netmask which allows 10.10.8.0/24, block everything else:

```
config router access-list
    edit "standard"
        config rule
            edit 1
                set prefix 10.10.8.0 255.255.255.0
            next
        end
    next
end
```

When this access-list is used as a BGP filter, routes that do not match the permit rule are not allowed.

Access-lists can also use a Cisco-style wildcard mask. A wildcard mask defines which bits must match and which bits can be ignored.

A `0` bit means: this bit must match.\
A `1` bit means: this bit can be anything.

```
config router access-list
    edit "cisco_wildcard"
        config rule
            edit 1
                unset prefix
                set wildcard 10.10.8.1 0.0.255.0
            next
            edit 2
                unset prefix
                set wildcard 172.16.0.0 0.0.15.255
            next
        end
    next
end
```

Rule 1 allows, 10.10.<0-255>1

Rule 2 allows, 172.16.<0-15>.<0-255>

