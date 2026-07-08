# Lab 8: Route-maps and local preference

## Objective

In this lab, we use a route-map to change the BGP local preference attribute.\
We match a specific route with a prefix-list and then apply a higher local preference to it.\
This allows us to influence the BGP best path selection process.\
At the end of the lab, R1 should prefer the path through R3 instead of R2.

## Topology

<figure><img src="https://github.com/sanderzegers/technotes/raw/refs/heads/undefined/fortigate-bgp/assets/topologies/exports/bgp-lab-master-Lab6.svg" alt=""><figcaption></figcaption></figure>

<table><thead><tr><th width="111.4166259765625">Router</th><th width="95.8333740234375" align="right">AS</th><th width="182.75">Interface IP</th><th width="201.5">BGP Neighbor</th><th>Local Networks</th></tr></thead><tbody><tr><td>R1</td><td align="right">65001</td><td>R1-R2: <code>172.18.12.0/31</code><br>R1-R3: <code>172.18.13.0/31</code></td><td>R2: <code>172.18.12.1</code><br>R3: <code>172.18.13.1</code></td><td>-</td></tr><tr><td>R2</td><td align="right">65002</td><td>R1-R2: <code>172.18.12.1/31</code><br>R2-R4: <code>172.18.24.0/31</code></td><td>R1: <code>172.18.12.0</code><br>R4: <code>172.18.24.1</code></td><td>-</td></tr><tr><td>R3</td><td align="right">65003</td><td>R1-R3: <code>172.18.13.1/31</code><br>R3-R4: <code>172.18.34.0/31</code></td><td>R1: <code>172.18.13.0</code><br>R4: <code>172.18.34.1</code></td><td>-</td></tr><tr><td>R4</td><td align="right">65004</td><td>R2-R4: <code>172.18.24.1/31</code><br>R3-R4: <code>172.18.34.1/31</code></td><td>R2: <code>172.18.24.0</code><br>R3: <code>172.18.34.0</code></td><td><code>10.10.4.0/24</code></td></tr></tbody></table>

## Packet Captures

<table><thead><tr><th width="174">PCAP File</th><th>Description</th><th>What to look for</th></tr></thead><tbody><tr><td></td><td></td><td></td></tr><tr><td></td><td></td><td></td></tr></tbody></table>

## Path Selection

Let's reset the lab, and configure the same settings as the previous lab 7:

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

We now want to control which route becomes the preferred route.

Let's verify that R2 is still the preferred route for the 10.10.4.0/24 on R1:

<pre><code>BGP-LAB (R1) # get router info bgp network 10.10.4.0/24
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

For this we nee d to configure route maps. So let's introduce them.

## Route Maps

Route-maps are used to control what happens to routes when they are received, advertised, or redistributed.

A route-map works like a small policy. It can **match** specific routes and then **set** or change something for those routes. For example, a route-map can match a prefix like `10.10.4.0/24` and then change a BGP attribute such as local preference.

Route-maps are often used together with prefix-lists or access-lists. The prefix-list defines **which routes** should match, and the route-map defines **what should happen** to those routes.

In this lab, we use a route-map to modify the BGP local preference attribute. This allows us to influence which BGP path is preferred without changing the network topology itself.

Let's configure a route map, which sets the local preference value to 200 for the 10.10.4.0/24 range:

```
config vdom
edit R1
config router prefix-list
    edit "PL_R4_LAN"
        config rule
            edit 1
                set action permit
                set prefix 10.10.4.0 255.255.255.0
                unset ge
                unset le
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
        end
    next
end
```

Now let's apply the route map to neighbor R3:

```
config router bgp
    config neighbor
        edit "172.18.13.1"
            set route-map-in "RM_PREF_R3_IN"
        next
    end
end
```

Let's refresh the received routes:

```
sudo R1 execute router clear bgp ip 172.18.13.1 soft in
```

<pre><code>BGP-LAB (R1) # get router info bgp network 10.10.4.0/24
VRF 0 BGP routing table entry for 10.10.4.0/24
Paths: (2 available, best #1, table Default-IP-Routing-Table)
  Advertised to non peer-group peers:
   172.18.12.1
  Original VRF 0
  65003 65004
    172.18.13.1 from 172.18.13.1 (3.3.3.3)
<strong>      Origin IGP distance 20 metric 0, localpref 200, valid, external, best
</strong>      Last update: Wed Jul  8 12:54:44 2026

  Original VRF 0
  65002 65004
    172.18.12.1 from 172.18.12.1 (2.2.2.2)
      Origin IGP distance 20 metric 0, localpref 100, valid, external
      Last update: Tue Jul  7 14:29:25 2026
</code></pre>

Verify that localpref is set to 200, and the route became the best route.

Let's take the interface between R1 and R3 down, verify, and re-enable the interface.

```
config system interface
edit R1R3-1-0
set status down
end
```

```
BGP-LAB (R1) # get router info bgp network 10.10.4.0/24
VRF 0 BGP routing table entry for 10.10.4.0/24
Paths: (1 available, best #1, table Default-IP-Routing-Table)
  Advertised to non peer-group peers:
   172.18.12.1
  Original VRF 0
  65002 65004
    172.18.12.1 from 172.18.12.1 (2.2.2.2)
      Origin IGP distance 20 metric 0, localpref 100, valid, external, best
      Last update: Tue Jul  7 14:29:25 2026

```

```
config system interface
edit R1R3-1-0
set status up
end
```

```
BGP-LAB (R1) # get router info bgp network 10.10.4.0/24
VRF 0 BGP routing table entry for 10.10.4.0/24
Paths: (2 available, best #1, table Default-IP-Routing-Table)
  Advertised to non peer-group peers:
   172.18.12.1 172.18.13.1
  Original VRF 0
  65003 65004
    172.18.13.1 from 172.18.13.1 (3.3.3.3)
      Origin IGP distance 20 metric 0, localpref 200, valid, external, best
      Last update: Wed Jul  8 12:58:59 2026

  Original VRF 0
  65002 65004
    172.18.12.1 from 172.18.12.1 (2.2.2.2)
      Origin IGP distance 20 metric 0, localpref 100, valid, external
      Last update: Tue Jul  7 14:29:25 2026
```

## Summary

In this lab we used a route-map to modify the BGP local preference attribute. Local preference is used to influence which outbound path is preferred by the local AS. A higher local preference is preferred.

We matched the route `10.10.4.0/24` with a prefix-list, used a route-map to set its local preference to `200`, and applied that route-map inbound on the R1 neighbor toward R3. After refreshing BGP, R1 selected the path via R3 instead of the path via R2.

This lab shows that route-maps are not only used for filtering routes. They can also be used to change BGP attributes and influence the BGP best path selection process.
