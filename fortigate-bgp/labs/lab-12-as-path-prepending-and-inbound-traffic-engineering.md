# Lab 12: AS-Path Prepending and Inbound Traffic Engineering

## Objective

In this lab, you will use AS-path prepending on R4 to make one advertisement less preferred. R4 will advertise `10.10.4.0/24` normally through R2 and with additional copies of AS 65004 through R3. You will verify that R1 prefers the shorter AS path through R2, test how local preference can override AS-path length, and confirm that the prepended path remains available as a backup.

{% hint style="info" %}
AS-path prepending is commonly described as inbound traffic engineering because R4 changes its advertisements to influence how remote routers send traffic toward R4. It is a preference signal, not a guarantee. A remote network can apply a higher-priority policy such as local preference.
{% endhint %}

## Topology

This lab reuses the topology from Labs 7 through 11. R4 advertises the same prefixes through R2 and R3. Without policy, R1 receives two paths of equal AS-path length. In this lab, R4 makes the path through R3 longer for only `10.10.4.0/24`.

<figure><img src="https://github.com/sanderzegers/technotes/raw/refs/heads/undefined/fortigate-bgp/assets/topologies/exports/bgp-lab-master-Lab6.svg" alt=""><figcaption></figcaption></figure>

| Router |    AS | Router ID | Interfaces                                                                      | BGP neighbors                                                       | Local networks                                                 |
| ------ | ----: | --------- | ------------------------------------------------------------------------------- | ------------------------------------------------------------------- | -------------------------------------------------------------- |
| R1     | 65001 | `1.1.1.1` | <p>R1-R2: <code>172.18.12.0/31</code><br>R1-R3: <code>172.18.13.0/31</code></p> | <p>R2: <code>172.18.12.1</code><br>R3: <code>172.18.13.1</code></p> | <p><code>10.10.1.0/24</code><br><code>172.17.0.1/32</code></p> |
| R2     | 65002 | `2.2.2.2` | <p>R1-R2: <code>172.18.12.1/31</code><br>R2-R4: <code>172.18.24.0/31</code></p> | <p>R1: <code>172.18.12.0</code><br>R4: <code>172.18.24.1</code></p> | -                                                              |
| R3     | 65003 | `3.3.3.3` | <p>R1-R3: <code>172.18.13.1/31</code><br>R3-R4: <code>172.18.34.0/31</code></p> | <p>R1: <code>172.18.13.0</code><br>R4: <code>172.18.34.1</code></p> | -                                                              |
| R4     | 65004 | `4.4.4.4` | <p>R2-R4: <code>172.18.24.1/31</code><br>R3-R4: <code>172.18.34.1/31</code></p> | <p>R2: <code>172.18.24.0</code><br>R3: <code>172.18.34.0</code></p> | <p><code>10.10.4.0/24</code><br><code>172.17.0.4/32</code></p> |

## Packet Captures

| PCAP File | Description | What to look for |
| --------- | ----------- | ---------------- |
|           |             |                  |

## AS-Path Prepending

The `AS_PATH` attribute records the autonomous systems through which a BGP route has been advertised. When the earlier best-path attributes are equal, BGP normally prefers the path containing fewer AS numbers.

An AS can make one path look less attractive by adding extra copies of its own AS number to an outbound advertisement. This is called AS-path prepending.

In this lab, the normal paths received by R1 are:

| Path   | Next hop      | AS path       | AS-path length |
| ------ | ------------- | ------------- | -------------- |
| Via R2 | `172.18.12.1` | `65002 65004` | 2              |
| Via R3 | `172.18.13.1` | `65003 65004` | 2              |

R4 will prepend two additional copies of AS 65004 to its advertisement toward R3. After R3 advertises that route to R1, the paths will be:

| Path   | AS path                   | AS-path length |
| ------ | ------------------------- | -------------- |
| Via R2 | `65002 65004`             | 2              |
| Via R3 | `65003 65004 65004 65004` | 4              |

The repeated AS numbers do not create a loop at R1 because R1 is in AS 65001. They increase the path length and make the route less preferred. A router in AS 65004 would reject either path because its own AS is already present, which is normal BGP loop prevention.

## Baseline Configuration

Restore the lab baseline and configure the four-router eBGP topology. BGP multipath from Lab 11 is not required for this lab and should be disabled on R1.

```
config vdom
    edit R1
        config router bgp
            set as 65001
            set router-id 1.1.1.1
            set ebgp-multipath disable
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

Neighbor    V         AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
172.18.12.1 4      65002      12      13        2    0    0 00:04:21        2
172.18.13.1 4      65003      12      13        2    0    0 00:04:18        2

Total number of neighbors 2
```

## Exercise 1: Observe the Equal AS Paths

Inspect `10.10.4.0/24` on R1 before applying any policy:

<pre><code>BGP-LAB (R1) # get router info bgp network 10.10.4.0/24
VRF 0 BGP routing table entry for 10.10.4.0/24
Paths: (2 available, best #2, table Default-IP-Routing-Table)
  Advertised to non peer-group peers:
   172.18.12.1
  Original VRF 0
  65002 65004
    172.18.12.1 from 172.18.12.1 (2.2.2.2)
      Origin IGP distance 20 metric 0, localpref 100, valid, external
      Last update: Fri Aug  7 06:31:40 2026

  Original VRF 0
  65003 65004
    172.18.13.1 from 172.18.13.1 (3.3.3.3)
      Origin IGP distance 20 metric 0, localpref 100, valid, external, best
      Last update: Fri Aug  7 06:30:26 2026</code></pre>

Both AS paths contain two AS numbers. With weight, local preference, AS-path length, origin, MED, and the other earlier attributes equal, BGP continues to later tie breakers. This example selects the path through R2, but your FortiGate may initially select R3 depending on session age and the remaining tie breakers.

Verify the installed route:

```
BGP-LAB (R1) # get router info routing-table details 10.10.4.0/24

Routing table for VRF=0
Routing entry for 10.10.4.0/24
  Known via "bgp", distance 20, metric 0, best
  Last update 00:04:50 ago
  * vrf 0 172.18.13.1 priority 1 (recursive is directly connected, R1R3-1-0)
```

## Exercise 2: Prepend the Path Through R3

Create a prefix-list on R4 that matches only `10.10.4.0/24`. Then create an outbound route-map that adds two copies of AS 65004 to the matching route.

<pre><code>config vdom
    edit R4
        config router prefix-list
            edit "PL_PREPEND_R3"
                config rule
                    edit 10
<strong>                        set prefix 10.10.4.0 255.255.255.0
</strong>                    next
                end
            next
        end
        config router route-map
            edit "RM_PREPEND_R3_OUT"
                config rule
                    edit 10
<strong>                        set match-ip-address "PL_PREPEND_R3"
</strong><strong>                        set set-aspath-action prepend
</strong><strong>                        set set-aspath "65004 65004"
</strong>                    next
<strong>                    edit 20
</strong><strong>                    next
</strong>                end
            next
        end
        config router bgp
            config neighbor
                edit "172.18.34.0"
<strong>                    set route-map-out "RM_PREPEND_R3_OUT"
</strong>                next
            end
        end
    next
end
</code></pre>

Rule 10 matches `10.10.4.0/24` and prepends AS 65004 twice. FortiGate also adds the normal local AS when sending the route to an eBGP neighbor, so R3 receives three copies of AS 65004.

Rule 20 permits routes that do not match rule 10 without changing them. Without this rule, the route-map's implicit deny would prevent other routes, such as `172.17.0.4/32`, from being advertised to R3.

Apply the changed outbound policy with a soft refresh:

```
sudo R4 execute router clear bgp ip 172.18.34.0 soft out
```

Verify what R4 advertises to R3:

```
BGP-LAB (R4) # get router info bgp neighbors 172.18.34.0 advertised-routes

     Network          Next Hop            Metric LocPrf Weight RouteTag Path
 *>  10.10.4.0/24     172.18.34.1              0         32768        0 65004 65004 i
 *>  172.17.0.4/32    172.18.34.1              0         32768        0 i
```

The advertised-routes view shows the two AS numbers inserted by the route-map. The receiving eBGP path also contains the normal local AS added during advertisement.

Now inspect the route on R1:

<pre><code>BGP-LAB (R1) # get router info bgp network 10.10.4.0/24
VRF 0 BGP routing table entry for 10.10.4.0/24
Paths: (2 available, best #2, table Default-IP-Routing-Table)
  Original VRF 0
<strong>  65003 65004 65004 65004
</strong>    172.18.13.1 from 172.18.13.1 (3.3.3.3)
      Origin IGP distance 20 metric 0, localpref 100, valid, external

  Original VRF 0
<strong>  65002 65004
</strong>    172.18.12.1 from 172.18.12.1 (2.2.2.2)
<strong>      Origin IGP distance 20 metric 0, localpref 100, valid, external, best
</strong></code></pre>

R1 now prefers the path through R2 because its AS path is shorter.

Confirm that the route-map changed only the selected prefix:

```
BGP-LAB (R1) # get router info bgp network 172.17.0.4/32

Paths: (2 available, best #2, table Default-IP-Routing-Table)
  65003 65004
    172.18.13.1 from 172.18.13.1 (3.3.3.3)
  65002 65004
    172.18.12.1 from 172.18.12.1 (2.2.2.2)
```

Both paths to `172.17.0.4/32` still have an AS-path length of two.

## Exercise 3: Override AS-Path Length with Local Preference

AS-path length is not the first step in the best-path algorithm. Weight and local preference are evaluated before it. To demonstrate this, configure R1 to assign local preference 200 to `10.10.4.0/24` when it is received from R3.

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

Refresh the routes received from R3:

```
sudo R1 execute router clear bgp ip 172.18.13.1 soft in
```

Verify the result:

<pre><code>BGP-LAB (R1) # get router info bgp network 10.10.4.0/24
VRF 0 BGP routing table entry for 10.10.4.0/24
Paths: (2 available, best #1, table Default-IP-Routing-Table)
  Original VRF 0
  65003 65004 65004 65004
    172.18.13.1 from 172.18.13.1 (3.3.3.3)
<strong>      Origin IGP distance 20 metric 0, localpref 200, valid, external, best
</strong>
  Original VRF 0
  65002 65004
    172.18.12.1 from 172.18.12.1 (2.2.2.2)
<strong>      Origin IGP distance 20 metric 0, localpref 100, valid, external
</strong></code></pre>

The longer path through R3 wins because its local preference is higher. This demonstrates why AS-path prepending cannot guarantee how another autonomous system will route traffic.

Remove the local-preference policy and refresh R3 again:

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

```
sudo R1 execute router clear bgp ip 172.18.13.1 soft in
```

R1 should return to the shorter path through R2.

## Exercise 4: Test the Prepended Backup Path

AS-path prepending makes a path less preferred; it does not remove the path. Disable the link between R1 and R2:

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

Verify the BGP table and routing table:

<pre><code>BGP-LAB (R1) # get router info bgp network 10.10.4.0/24
VRF 0 BGP routing table entry for 10.10.4.0/24
Paths: (1 available, best #1, table Default-IP-Routing-Table)
  Original VRF 0
  65003 65004 65004 65004
    172.18.13.1 from 172.18.13.1 (3.3.3.3)
<strong>      Origin IGP distance 20 metric 0, localpref 100, valid, external, best
</strong>
BGP-LAB (R1) # get router info routing-table details 10.10.4.0/24

Routing table for VRF=0
Routing entry for 10.10.4.0/24
  Known via "bgp", distance 20, metric 0, best
<strong>  * 172.18.13.1, via R1R3-1-0
</strong></code></pre>

The prepended path becomes best because it is the only remaining valid path.

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

After the R1-R2 BGP session is established again, R1 should return to the shorter path through R2.

## Exercise 5: Remove AS-Path Prepending

Remove the outbound route-map from the R4 neighbor toward R3:

```
config vdom
    edit R4
        config router bgp
            config neighbor
                edit "172.18.34.0"
                    unset route-map-out
                next
            end
        end
    next
end
```

Refresh the outbound advertisements and inspect the route on R1:

```
sudo R4 execute router clear bgp ip 172.18.34.0 soft out
```

```
BGP-LAB (R1) # get router info bgp network 10.10.4.0/24

Paths: (2 available, best #2, table Default-IP-Routing-Table)
  65003 65004
    172.18.13.1 from 172.18.13.1 (3.3.3.3)
  65002 65004
    172.18.12.1 from 172.18.12.1 (2.2.2.2)
```

Both paths should have an AS-path length of two again. The route-map and prefix-list can remain configured without affecting BGP because the route-map is no longer attached to a neighbor.

## Summary

In this lab, you used an outbound route-map on R4 to prepend two additional copies of AS 65004 to `10.10.4.0/24` when advertising it toward R3. R1 therefore received a shorter path through R2 and a longer path through R3, and selected the shorter path through R2.

You used a prefix-list to apply prepending to only one route and added a second route-map rule to permit unmatched routes. You also confirmed that a higher local preference on R1 overrides the shorter AS path.

Finally, you disabled the preferred link and verified that the prepended path remained available as a backup. AS-path prepending influences how remote networks reach an AS, but it does not remove alternate paths or override policies that are evaluated earlier in the BGP best-path process.

## Links

{% embed url="https://docs.fortinet.com/document/fortigate/7.4.8/administration-guide/535228/route-maps" %}

