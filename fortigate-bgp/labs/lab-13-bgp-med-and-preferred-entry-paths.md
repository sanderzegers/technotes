# Lab 13: BGP MED and Preferred Entry Paths

## Objective

In this lab, you will use the BGP Multi-Exit Discriminator (MED) to influence which entry path a neighboring AS prefers. R2 and R3 will advertise the same route from the same transit AS, but with different MED values. You will verify that R1 prefers the lower MED, demonstrate that local preference can override MED, and confirm that the less-preferred path remains available as a backup.

{% hint style="info" %}
MED is a suggestion sent to a neighboring AS. A lower MED is preferred. The neighboring AS can override that suggestion with a higher-priority policy such as local preference.
{% endhint %}

## Topology

This lab uses the same four-router diamond topology as Labs 7 through 12, with one important change: **R2 and R3 now belong to the same transit AS, AS 65023**. This allows R1 to compare the MED values received over the two paths.

<figure><img src="https://github.com/sanderzegers/technotes/raw/refs/heads/undefined/fortigate-bgp/assets/topologies/exports/bgp-lab-master-Lab6.svg" alt=""><figcaption></figcaption></figure>

| Router |    AS | Router ID | Interfaces                                                                      | BGP neighbors                                                       | Local networks                                                 |
| ------ | ----: | --------- | ------------------------------------------------------------------------------- | ------------------------------------------------------------------- | -------------------------------------------------------------- |
| R1     | 65001 | `1.1.1.1` | <p>R1-R2: <code>172.18.12.0/31</code><br>R1-R3: <code>172.18.13.0/31</code></p> | <p>R2: <code>172.18.12.1</code><br>R3: <code>172.18.13.1</code></p> | <p><code>10.10.1.0/24</code><br><code>172.17.0.1/32</code></p> |
| R2     | 65023 | `2.2.2.2` | <p>R1-R2: <code>172.18.12.1/31</code><br>R2-R4: <code>172.18.24.0/31</code></p> | <p>R1: <code>172.18.12.0</code><br>R4: <code>172.18.24.1</code></p> | -                                                              |
| R3     | 65023 | `3.3.3.3` | <p>R1-R3: <code>172.18.13.1/31</code><br>R3-R4: <code>172.18.34.0/31</code></p> | <p>R1: <code>172.18.13.0</code><br>R4: <code>172.18.34.1</code></p> | -                                                              |
| R4     | 65004 | `4.4.4.4` | <p>R2-R4: <code>172.18.24.1/31</code><br>R3-R4: <code>172.18.34.1/31</code></p> | <p>R2: <code>172.18.24.0</code><br>R3: <code>172.18.34.0</code></p> | <p><code>10.10.4.0/24</code><br><code>172.17.0.4/32</code></p> |

R2 and R3 belong to the same AS but do not peer with each other in this simplified topology. \
Each router independently carries routes between R1 and R4. In a production network, border routers in the same AS would normally exchange routes using iBGP or route reflectors, while an IGP provides internal reachability.

## Packet Captures

| PCAP File | Description | What to look for |
| --------- | ----------- | ---------------- |
|           |             |                  |

## Multi-Exit Discriminator

MED is an optional, non-transitive BGP path attribute. It is used to suggest which connection a neighboring AS should use when entering the advertising AS.

The important MED rules for this lab are:

* A lower MED is preferred.
* MED is evaluated after weight, local preference, AS-path length, and origin.
* By default, MED is normally compared only between paths learned from the same neighboring AS.
* MED is a suggestion and does not force the neighboring AS to follow it.

{% hint style="warning" %}
In Labs 7 through 12, R2 and R3 used different AS numbers. MED would not normally be compared between those two paths. In this lab, both routers use AS 65023 so the comparison is meaningful.
{% endhint %}

R2 will advertise `10.10.4.0/24` to R1 with MED 50. R3 will advertise the same route with MED 200:

| Path   | Advertising AS | AS path       | MED | Result         |
| ------ | -------------- | ------------- | --: | -------------- |
| Via R2 | 65023          | `65023 65004` |  50 | Preferred      |
| Via R3 | 65023          | `65023 65004` | 200 | Alternate path |

## Baseline Configuration

Restore the lab baseline and configure the four-router topology. Make sure that policies from earlier labs are no longer applied and that BGP multipath is disabled on R1.

```
config vdom
    edit R1
        config router bgp
            set as 65001
            set router-id 1.1.1.1
            set ebgp-multipath disable
            config neighbor
                edit "172.18.12.1"
                    set remote-as 65023
                next
                edit "172.18.13.1"
                    set remote-as 65023
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
            set as 65023
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
            set as 65023
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
                    set remote-as 65023
                next
                edit "172.18.34.0"
                    set remote-as 65023
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

Verify that both sessions on R1 are established:

```
BGP-LAB (R1) # get router info bgp summary

VRF 0 BGP router identifier 1.1.1.1, local AS number 65001
BGP table version is 2
5 BGP AS-PATH entries
0 BGP community entries

Neighbor    V         AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
172.18.12.1 4      65023       5       7        2    0    0 00:02:15        2
172.18.13.1 4      65023       6       7        1    0    0 00:03:21        2

Total number of neighbors 2


```

## Exercise 1: Observe the Equal Paths

Inspect `10.10.4.0/24` on R1 before configuring MED:

<pre><code>BGP-LAB (R1) # get router info bgp network 10.10.4.0/24
VRF 0 BGP routing table entry for 10.10.4.0/24
Paths: (2 available, best #2, table Default-IP-Routing-Table)
  Advertised to non peer-group peers:
   172.18.12.1
  Original VRF 0
<strong>  65023 65004
</strong>    172.18.12.1 from 172.18.12.1 (2.2.2.2)
      Origin IGP distance 20 metric 0, localpref 100, valid, external
      Last update: Fri Aug  7 07:16:01 2026

  Original VRF 0
<strong>  65023 65004
</strong>    172.18.13.1 from 172.18.13.1 (3.3.3.3)
      Origin IGP distance 20 metric 0, localpref 100, valid, external, best
      Last update: Fri Aug  7 07:14:59 2026
</code></pre>

Both paths come from AS 65023, have the same AS path, and have the default MED of 0. BGP continues to later tie breakers to select one best path. Your FortiGate may select the path through R2 or R3 at this stage.

## Exercise 2: Advertise Different MED Values

Create a prefix-list on R2 that matches only `10.10.4.0/24`. Create an outbound route-map that sets MED 50, then apply it toward R1.

<pre><code>config vdom
    edit R2
        config router prefix-list
            edit "PL_R4_LAN"
                config rule
                    edit 10
<strong>                        set prefix 10.10.4.0 255.255.255.0
</strong>                    next
                end
            next
        end
        config router route-map
            edit "RM_MED_R1_OUT"
                config rule
                    edit 10
                        set match-ip-address "PL_R4_LAN"
<strong>                        set set-metric 50
</strong>                    next
                    edit 20
                    next
                end
            next
        end
        config router bgp
            config neighbor
                edit "172.18.12.0"
<strong>                    set route-map-out "RM_MED_R1_OUT"
</strong>                next
            end
        end
    next
end
</code></pre>

Configure the same policy on R3, but set MED 200:

<pre><code>config vdom
    edit R3
        config router prefix-list
            edit "PL_R4_LAN"
                config rule
                    edit 10
<strong>                        set prefix 10.10.4.0 255.255.255.0
</strong>                    next
                end
            next
        end
        config router route-map
            edit "RM_MED_R1_OUT"
                config rule
                    edit 10
                        set match-ip-address "PL_R4_LAN"
<strong>                        set set-metric 200
</strong>                    next
                    edit 20
                    next
                end
            next
        end
        config router bgp
            config neighbor
                edit "172.18.13.0"
<strong>                    set route-map-out "RM_MED_R1_OUT"
</strong>                next
            end
        end
    next
end
</code></pre>

Rule 20 permits routes that do not match `10.10.4.0/24` without changing them. Without this rule, the route-map's implicit deny would prevent other routes from being advertised.

Refresh the advertisements toward R1:

```
sudo R2 execute router clear bgp ip 172.18.12.0 soft out
sudo R3 execute router clear bgp ip 172.18.13.0 soft out
```

Verify the result on R1:

```
BGP-LAB (R1) #  get router info bgp network 10.10.4.0/24
VRF 0 BGP routing table entry for 10.10.4.0/24
Paths: (2 available, best #1, table Default-IP-Routing-Table)
  Advertised to non peer-group peers:
   172.18.12.1 172.18.13.1
  Original VRF 0
  65023 65004
    172.18.12.1 from 172.18.12.1 (2.2.2.2)
      Origin IGP distance 20 metric 50, localpref 100, valid, external, best
      Last update: Fri Aug  7 07:23:57 2026

  Original VRF 0
  65023 65004
    172.18.13.1 from 172.18.13.1 (3.3.3.3)
      Origin IGP distance 20 metric 200, localpref 100, valid, external
      Last update: Fri Aug  7 07:23:43 2026
```

R1 prefers the path through R2 because MED 50 is lower than MED 200.

Confirm that the route-map changed only the selected prefix:

<pre><code>BGP-LAB (R1) # get router info bgp network 172.17.0.4/32
VRF 0 BGP routing table entry for 172.17.0.4/32
Paths: (2 available, best #2, table Default-IP-Routing-Table)
  Advertised to non peer-group peers:
   172.18.12.1
  Original VRF 0
  65023 65004
    172.18.12.1 from 172.18.12.1 (2.2.2.2)
<strong>      Origin IGP distance 20 metric 0, localpref 100, valid, external
</strong>      Last update: Fri Aug  7 07:23:57 2026

  Original VRF 0
  65023 65004
    172.18.13.1 from 172.18.13.1 (3.3.3.3)
<strong>      Origin IGP distance 20 metric 0, localpref 100, valid, external, best
</strong>      Last update: Fri Aug  7 07:23:43 2026
</code></pre>

Both paths to `172.17.0.4/32` should still show metric 0.

## Exercise 3: Change the Preferred Entry Path

Change the MED values so that the path through R3 becomes preferred:

```
config vdom
    edit R2
        config router route-map
            edit "RM_MED_R1_OUT"
                config rule
                    edit 10
                        set set-metric 200
                    next
                end
            next
        end
    next
    edit R3
        config router route-map
            edit "RM_MED_R1_OUT"
                config rule
                    edit 10
                        set set-metric 50
                    next
                end
            next
        end
    next
end
```

Refresh both outbound advertisements again:

```
sudo R2 execute router clear bgp ip 172.18.12.0 soft out
sudo R3 execute router clear bgp ip 172.18.13.0 soft out
```

R1 should now show metric 200 through R2 and metric 50 through R3. The path through R3 should be selected as best.

<pre><code>BGP-LAB (R1) # get  router info bgp network 
VRF 0 BGP table version is 4, local router ID is 1.1.1.1
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              S Stale
Origin codes: i - IGP, e - EGP, ? - incomplete

   Network          Next Hop            Metric     LocPrf Weight RouteTag Path
*> 10.10.1.0/24     0.0.0.0                       100  32768        0 i &#x3C;-/1>
<strong>*> 10.10.4.0/24     172.18.12.1     50                     0        0 65023 65004 i &#x3C;-/1>
</strong><strong>*                   172.18.13.1     200                    0        0 65023 65004 i &#x3C;-/->
</strong>*> 172.17.0.1/32    0.0.0.0                       100  32768        0 i &#x3C;-/1>
*  172.17.0.4/32    172.18.12.1     0                      0        0 65023 65004 i &#x3C;-/->
*>                  172.18.13.1     0                      0        0 65023 65004 i &#x3C;-/1>

Total number of prefixes 4


BGP-LAB (R1) # get  router info bgp network 10.10.4.0/24
VRF 0 BGP routing table entry for 10.10.4.0/24
Paths: (2 available, best #1, table Default-IP-Routing-Table)
  Advertised to non peer-group peers:
   172.18.13.1
  Original VRF 0
  65023 65004
    172.18.12.1 from 172.18.12.1 (2.2.2.2)
<strong>      Origin IGP distance 20 metric 50, localpref 100, valid, external, best
</strong>      Last update: Fri Aug  7 07:23:57 2026

  Original VRF 0
  65023 65004
    172.18.13.1 from 172.18.13.1 (3.3.3.3)
<strong>      Origin IGP distance 20 metric 200, localpref 100, valid, external
</strong>      Last update: Fri Aug  7 07:23:43 2026

</code></pre>

## Exercise 4: Override MED with Local Preference

Local preference is evaluated before MED. Configure R1 to assign local preference 200 to `10.10.4.0/24` when it is received from R2.

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
            edit "RM_PREF_R2_IN"
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
                edit "172.18.12.1"
                    set route-map-in "RM_PREF_R2_IN"
                next
            end
        end
    next
end
```

Refresh the routes received from R2:

```
sudo R1 execute router clear bgp ip 172.18.12.1 soft in
```

Verify the result:

<pre><code>BGP-LAB (R1) # get router info bgp network 10.10.4.0/24
  65023 65004
    172.18.12.1 from 172.18.12.1 (2.2.2.2)
<strong>      Origin IGP distance 20 metric 200, localpref 200, valid, external, best
</strong>
  65023 65004
    172.18.13.1 from 172.18.13.1 (3.3.3.3)
<strong>      Origin IGP distance 20 metric 50, localpref 100, valid, external
</strong></code></pre>

The R2 path wins even though it has the higher MED. Its local preference is evaluated first and is higher than the local preference of the R3 path.

Remove the local-preference policy and refresh the neighbor:

```
config vdom
    edit R1
        config router bgp
            config neighbor
                edit "172.18.12.1"
                    unset route-map-in
                next
            end
        end
    next
end
```

```
sudo R1 execute router clear bgp ip 172.18.12.1 soft in
```

R1 should return to the lower-MED path through R3.

<pre><code>BGP-LAB (R1) # get router info bgp network 10.10.4.0/24
VRF 0 BGP routing table entry for 10.10.4.0/24
Paths: (2 available, best #1, table Default-IP-Routing-Table)
  Advertised to non peer-group peers:
   172.18.13.1
  Original VRF 0
  65023 65004
    172.18.12.1 from 172.18.12.1 (2.2.2.2)
<strong>      Origin IGP distance 20 metric 200, localpref 200, valid, external, best
</strong>      Last update: Fri Aug  7 07:36:02 2026

  Original VRF 0
  65023 65004
    172.18.13.1 from 172.18.13.1 (3.3.3.3)
      Origin IGP distance 20 metric 200, localpref 100, valid, external
      Last update: Fri Aug  7 07:23:43 2026
</code></pre>

## Exercise 5: Test the Alternate Path

Disable the preferred link between R1 and R3:

```
config vdom
    edit R1
        config system interface
            edit "R1R3-1-0"
                set status down
            next
        end
    next
end
```

Verify that the path through R2 becomes best even though it has the higher MED:

```
BGP-LAB (R1) # get router info bgp network 10.10.4.0/24
VRF 0 BGP routing table entry for 10.10.4.0/24
Paths: (1 available, best #1, table Default-IP-Routing-Table)
  Not advertised to any peer
  Original VRF 0
  65023 65004
    172.18.12.1 from 172.18.12.1 (2.2.2.2)
      Origin IGP distance 20 metric 200, localpref 200, valid, external, best
      Last update: Fri Aug  7 07:36:02 2026
```

MED makes a path less preferred; it does not remove the path.

Re-enable the link:

```
config vdom
    edit R1
        config system interface
            edit "R1R3-1-0"
                set status up
            next
        end
    next
end
```

After the BGP session is established again, the lower-MED path through R3 should become best.

## Summary

In this lab, R2 and R3 advertised the same route from AS 65023 with different MED values. R1 compared the MED values because both paths came from the same neighboring AS and selected the path with the lower MED.

You used outbound route-maps to set MED 50 through one entry point and MED 200 through the other. You then reversed the values and confirmed that the preferred path changed.

You also demonstrated that local preference is evaluated before MED and can override the neighboring AS's suggestion. Finally, you disabled the preferred path and verified that the higher-MED route remained available as a backup.

MED can influence which entry point a neighboring AS uses, but it is not a guarantee. It is most useful when multiple connections exist between the same two autonomous systems.

## Links

{% embed url="https://docs.fortinet.com/document/fortigate/7.4.4/administration-guide/134679/bgp-multi-exit-discriminator" %}
