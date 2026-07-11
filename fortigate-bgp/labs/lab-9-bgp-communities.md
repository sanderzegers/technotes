# Lab 9: BGP Communities

## Objective

In this lab, you will learn how to attach BGP communities to routes, advertise them to neighboring routers, and use them in route-maps to apply routing policy. Unlike the previous lab, where route-maps matched prefixes and directly modified route attributes, this lab introduces BGP communities as tags that can be attached to routes and used by other routers to make policy decisions.

## Topology

<figure><img src="https://github.com/sanderzegers/technotes/raw/refs/heads/undefined/fortigate-bgp/assets/topologies/exports/bgp-lab-master-Lab6.svg" alt=""><figcaption></figcaption></figure>

<table><thead><tr><th width="111.4166259765625">Router</th><th width="95.8333740234375" align="right">AS</th><th width="182.75">Interface IP</th><th width="201.5">BGP Neighbor</th><th>Local Networks</th></tr></thead><tbody><tr><td>R1</td><td align="right">65001</td><td>R1-R2: <code>172.18.12.0/31</code><br>R1-R3: <code>172.18.13.0/31</code></td><td>R2: <code>172.18.12.1</code><br>R3: <code>172.18.13.1</code></td><td>-</td></tr><tr><td>R2</td><td align="right">65002</td><td>R1-R2: <code>172.18.12.1/31</code><br>R2-R4: <code>172.18.24.0/31</code></td><td>R1: <code>172.18.12.0</code><br>R4: <code>172.18.24.1</code></td><td>-</td></tr><tr><td>R3</td><td align="right">65003</td><td>R1-R3: <code>172.18.13.1/31</code><br>R3-R4: <code>172.18.34.0/31</code></td><td>R1: <code>172.18.13.0</code><br>R4: <code>172.18.34.1</code></td><td>-</td></tr><tr><td>R4</td><td align="right">65004</td><td>R2-R4: <code>172.18.24.1/31</code><br>R3-R4: <code>172.18.34.1/31</code></td><td>R2: <code>172.18.24.0</code><br>R3: <code>172.18.34.0</code></td><td><p>R4-LAN0:<br><code>10.10.4.0/24</code></p><p>R4-LAN1:</p><p><code>10.10.40.0/24</code></p></td></tr></tbody></table>

## Packet Captures

<table><thead><tr><th width="174">PCAP File</th><th>Description</th><th>What to look for</th></tr></thead><tbody><tr><td><a href="https://github.com/sanderzegers/technotes/raw/refs/heads/undefined/fortigate-bgp/pcaps/Lab9/lab9-communities-soft-reset.pcapng">lab9-communities-soft-reset.pcapng</a></td><td>Policy routes applied to neighbors and soft reset</td><td>Update packets containing the community attribute.</td></tr><tr><td><a href="https://github.com/sanderzegers/technotes/raw/refs/heads/undefined/fortigate-bgp/pcaps/Lab9/lab9-no-export-community-applied.pcapng">lab9-no-export-community-applied.pcapng</a></td><td>Exercise 3. No export-policy applied.</td><td>Updates packets containing NO_EXPORT community, from R4 to R3 and R2.<br>Update messages to R1 to withdraw the 10.10.40.0/24 route.</td></tr></tbody></table>

## BGP Communities

Let's reset the lab, and configure the same settings as the previous lab 8:

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
        config system interface
            edit R4-LAN1
                set vdom "R4"
                set ip 10.10.40.1 255.255.255.0
                set allowaccess ping
                set type loopback
            next
        end
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
                    set prefix 10.10.40.0 255.255.255.0
                next
            end
        end
    next
end
```

Verify the baseline config:

<pre><code><strong>BGP-LAB (global) # sudo R4 get router info routing-table all
</strong>Codes: K - kernel, C - connected, S - static, R - RIP, B - BGP
       O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
       V - BGP VPNv4
       * - candidate default

Routing table for VRF=0
<strong>C       10.10.4.0/24 is directly connected, R4-LAN0
</strong><strong>C       10.10.40.0/24 is directly connected, R4-LAN1
</strong>C       172.17.0.4/32 is directly connected, R4-LO0
C       172.18.14.0/31 is directly connected, R1R4-1
C       172.18.24.0/31 is directly connected, R2R4-1
C       172.18.34.0/31 is directly connected, R3R4-1
C       172.19.0.0/24 is directly connected, R4NET1-0
<strong>BGP-LAB (global) # sudo R4 get router info bgp network 
</strong>VRF 0 BGP table version is 2, local router ID is 4.4.4.4
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              S Stale
Origin codes: i - IGP, e - EGP, ? - incomplete

   Network          Next Hop            Metric     LocPrf Weight RouteTag Path
<strong>*> 10.10.4.0/24     0.0.0.0                       100  32768        0 i &#x3C;-/1>
</strong><strong>*> 10.10.40.0/24    0.0.0.0                       100  32768        0 i &#x3C;-/1>
</strong>
Total number of prefixes 2


<strong>BGP-LAB (global) # sudo R1 get router info bgp network 
</strong>VRF 0 BGP table version is 2, local router ID is 1.1.1.1
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              S Stale
Origin codes: i - IGP, e - EGP, ? - incomplete

   Network          Next Hop            Metric     LocPrf Weight RouteTag Path
<strong>*  10.10.4.0/24     172.18.12.1     0                      0        0 65002 65004 i &#x3C;-/->
</strong><strong>*>                  172.18.13.1     0                      0        0 65003 65004 i &#x3C;-/1>
</strong><strong>*  10.10.40.0/24    172.18.13.1     0                      0        0 65003 65004 i &#x3C;-/->
</strong><strong>*>                  172.18.12.1     0                      0        0 65002 65004 i &#x3C;-/1>
</strong>
Total number of prefixes 2


<strong>BGP-LAB (global) # sudo R1 get router info routing-table bgp
</strong>Routing table for VRF=0
<strong>B       10.10.4.0/24 [20/0] via 172.18.13.1 (recursive is directly connected, R1R3-1-0), 00:03:52, [1/0]
</strong><strong>B       10.10.40.0/24 [20/0] via 172.18.12.1 (recursive is directly connected, R1R2-0), 00:00:52, [1/0]
</strong></code></pre>

### Exercise 1: Assigning BGP Communities

As a first step you will attach a BGP community to one of the routes advertised by R4 and send it to R2 and R3. You will then verify how the community appears in the BGP table before using it for routing policy.

{% hint style="info" %}
**BGP Communities**

A BGP community is a tag that can be attached to a route. Other routers can use this tag to identify the route and apply a routing policy.

Communities do not change route selection by themselves. They are simply labels that can be matched in route-maps to perform actions such as changing attributes or filtering routes.

Communities can be passed between routers and even across autonomous systems, making them a simple way to share routing information and policy decisions.
{% endhint %}

Let's create two route maps on R4:

```
config vdom
edit R4
    config router prefix-list
        edit "PL_10.10.4.0"
            config rule
                edit 1
                    set prefix 10.10.4.0 255.255.255.0
                next
            end
        next
    
        edit "PL_10.10.40.0"
            config rule
                edit 1
                    set prefix 10.10.40.0 255.255.255.0
                next
            end
        next
    end

    config router route-map
        edit "RM_SET_COMMUNITIES"
            config rule
                edit 10
                    set match-ip-address "PL_10.10.4.0"
                    set set-community "65004:100"
                next
                edit 20
                    set match-ip-address "PL_10.10.40.0"
                    set set-community "65004:200"
                next
            end
        next
    end
end
```

{% hint style="info" %}
A standard BGP community is a 32-bit value, usually written as `ASN:value`. Using the local AS number The first value is commonly the originating AS number, but this is a convention rather than a requirement.\
\
This lab uses standard BGP communities, which are 32-bit values. BGP also supports extended communities (64-bit values) and large communities (96-bit values), which provide additional flexibility and carry more information.
{% endhint %}

```
config vdom
edit R4
    config router bgp
        config neighbor
            edit "172.18.24.0"
                set route-map-out "RM_SET_COMMUNITIES"
            next
            edit "172.18.34.0"
                set route-map-out "RM_SET_COMMUNITIES"
            next
        end
    end
```

```
BGP-LAB (R4) # execute router clear bgp all  soft out
```

Verify that the route maps are applied on R4:

<pre><code><strong>BGP-LAB (R4) # get router info bgp route-map RM_SET_COMMUNITIES 
</strong>VRF 0 BGP table version is 1, local router ID is 4.4.4.4
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              S Stale
Origin codes: i - IGP, e - EGP, ? - incomplete

   Network          Next Hop            Metric     LocPrf Weight RouteTag Path
<strong>*> 10.10.4.0/24     0.0.0.0                       100  32768        0 i &#x3C;-/1>
</strong><strong>*> 10.10.40.0/24    0.0.0.0                       100  32768        0 i &#x3C;-/1>
</strong>
Total number of prefixes 2
</code></pre>

The community is added by an outbound route-map. FortiGate does not always display outbound communities in the local BGP table, so the easiest verification is often on the receiving router or with a packet capture.

<pre><code>BGP-LAB (R4) # sudo R2 get router info bgp network 10.10.4.0/24
VRF 0 BGP routing table entry for 10.10.4.0/24
Paths: (1 available, best #1, table Default-IP-Routing-Table)
  Advertised to non peer-group peers:
   172.18.12.0
  Original VRF 0
  65004
    172.18.24.1 from 172.18.24.1 (4.4.4.4)
      Origin IGP distance 20 metric 0, localpref 100, valid, external, best
<strong>      Community: 65004:100
</strong>      Last update: Sat Jul 11 06:57:52 2026



BGP-LAB (R4) # sudo R1 get router info bgp network 10.10.4.0/24
VRF 0 BGP routing table entry for 10.10.4.0/24
Paths: (2 available, best #2, table Default-IP-Routing-Table)
  Advertised to non peer-group peers:
   172.18.13.1
  Original VRF 0
  65003 65004
    172.18.13.1 from 172.18.13.1 (3.3.3.3)
      Origin IGP distance 20 metric 0, localpref 100, valid, external
<strong>      Community: 65004:100
</strong>      Last update: Sat Jul 11 06:57:36 2026

  Original VRF 0
  65002 65004
    172.18.12.1 from 172.18.12.1 (2.2.2.2)
      Origin IGP distance 20 metric 0, localpref 100, valid, external, best
<strong>      Community: 65004:100
</strong>      Last update: Sat Jul 11 06:57:58 2026



BGP-LAB (R4) # sudo R1 get router info bgp network 10.10.40.0/24
VRF 0 BGP routing table entry for 10.10.40.0/24
Paths: (2 available, best #2, table Default-IP-Routing-Table)
  Advertised to non peer-group peers:
   172.18.13.1
  Original VRF 0
  65003 65004
    172.18.13.1 from 172.18.13.1 (3.3.3.3)
      Origin IGP distance 20 metric 0, localpref 100, valid, external
<strong>      Community: 65004:200
</strong>      Last update: Sat Jul 11 06:57:36 2026

  Original VRF 0
  65002 65004
    172.18.12.1 from 172.18.12.1 (2.2.2.2)
      Origin IGP distance 20 metric 0, localpref 100, valid, external, best
<strong>      Community: 65004:200
</strong>      Last update: Sat Jul 11 06:57:58 2026


</code></pre>

### Exercise 2: Selecting Paths with Communities

In this exercise, you will use the communities assigned in the previous exercise to make R1 prefer a different path for each network advertised by R4.

First, create two community lists on R1. Each list matches one of the communities configured on R4.

Next, create two inbound route-maps:

* Routes received from R2 with community `65004:100` receive a local preference of `200`.
* Routes received from R3 with community `65004:200` receive a local preference of `200`.

This results in the following path-selection policy:

| R4 Prefix     | Community | Preferred Path on R1 |
| ------------- | --------- | -------------------- |
| 10.10.4.0/24  | 65004:100 | via R2               |
| 10.10.40.0/24 | 65004:200 | via R3               |

<pre><code>config vdom
edit R1
    config router community-list
        edit "CL_65004_100"
            config rule
                edit 10
                    set action permit
                    set match "65004:100"
                next
            end
        next
    
        edit "CL_65004_200"
            config rule
                edit 10
                    set action permit
                    set match "65004:200"
                next
            end
        next
    end
    
    config router route-map
        edit "RM_PREF_R2_IN"
            config rule
                edit 100
<strong>                    set match-community "CL_65004_100"
</strong><strong>                    set set-local-preference 200
</strong>                next
            end
        next
    
        edit "RM_PREF_R3_IN"
            config rule
                edit 100
<strong>                    set match-community "CL_65004_200"
</strong><strong>                    set set-local-preference 200
</strong>                next
            end
        next
    end
end
</code></pre>

Apply them to R1 neighbors and perform a soft inbound reset:

```
config vdom
edit "R1"
    config router bgp
        config neighbor
            edit "172.18.12.1"
                set route-map-in "RM_PREF_R2_IN"
            next
            edit "172.18.13.1"
                set route-map-in "RM_PREF_R3_IN"
            next
        end
    end
```

```
BGP-LAB (R1) # execute router clear bgp all soft in
```

Now verify the routing table:

```
BGP-LAB (R1) # get router info routing-table bgp 
Routing table for VRF=0
B       10.10.4.0/24 [20/0] via 172.18.12.1 (recursive is directly connected, R1R2-0), 00:00:18, [1/0]
B       10.10.40.0/24 [20/0] via 172.18.13.1 (recursive is directly connected, R1R3-1-0), 00:00:38, [1/0]
```

And the network tables:

<pre><code><strong>BGP-LAB (R1) # get router info bgp network 10.10.4.0/24
</strong>VRF 0 BGP routing table entry for 10.10.4.0/24
Paths: (1 available, best #1, table Default-IP-Routing-Table)
  Advertised to non peer-group peers:
   172.18.13.1
  Original VRF 0
  65002 65004
    172.18.12.1 from 172.18.12.1 (2.2.2.2)
<strong>      Origin IGP distance 20 metric 0, localpref 200, valid, external, best
</strong>      Community: 65004:100
      Last update: Sat Jul 11 07:35:02 2026



<strong>BGP-LAB (R1) # get router info bgp network 10.10.40.0/24
</strong>VRF 0 BGP routing table entry for 10.10.40.0/24
Paths: (1 available, best #1, table Default-IP-Routing-Table)
  Advertised to non peer-group peers:
   172.18.12.1
  Original VRF 0
  65003 65004
    172.18.13.1 from 172.18.13.1 (3.3.3.3)
<strong>      Origin IGP distance 20 metric 0, localpref 200, valid, external, best
</strong>      Community: 65004:200
      Last update: Sat Jul 11 07:34:42 2026

</code></pre>

The local preference of `200` was applied correctly. However, the BGP table now contains only one path for each network. The other path was rejected because the route-map did not contain a rule that permitted unmatched routes.

To keep the alternative paths as backups, add an empty permit rule to both route-maps:

<pre><code>config router route-map
    edit "RM_PREF_R2_IN"
        config rule
            edit 100
                set match-community "CL_65004_100"
                set set-local-preference 200
            next
<strong>            edit 200
</strong><strong>            next
</strong>        end
    next
    edit "RM_PREF_R3_IN"
        config rule
            edit 100
                set match-community "CL_65004_200"
                set set-local-preference 200
            next
<strong>            edit 200
</strong><strong>            next
</strong>        end
    next
end
</code></pre>

Rule `200` has no match conditions or actions. It therefore permits all remaining routes without changing their attributes.

Perform another soft inbound reset:

```
BGP-LAB (R1) # execute router clear bgp all soft in
```

<pre><code>BGP-LAB (R1) # get router info bgp network 10.10.4.0/24
VRF 0 BGP routing table entry for 10.10.4.0/24
Paths: (2 available, best #2, table Default-IP-Routing-Table)
  Advertised to non peer-group peers:
   172.18.13.1
  Original VRF 0
  65003 65004
<strong>    172.18.13.1 from 172.18.13.1 (3.3.3.3)
</strong>      Origin IGP distance 20 metric 0, localpref 100, valid, external
      Community: 65004:100
      Last update: Sat Jul 11 07:40:38 2026

  Original VRF 0
  65002 65004
<strong>    172.18.12.1 from 172.18.12.1 (2.2.2.2)
</strong><strong>      Origin IGP distance 20 metric 0, localpref 200, valid, external, best
</strong>      Community: 65004:100
      Last update: Sat Jul 11 07:40:50 2026



<strong>BGP-LAB (R1) # get router info bgp network 10.10.40.0/24
</strong>VRF 0 BGP routing table entry for 10.10.40.0/24
Paths: (2 available, best #2, table Default-IP-Routing-Table)
  Advertised to non peer-group peers:
   172.18.12.1
  Original VRF 0
  65002 65004
<strong>    172.18.12.1 from 172.18.12.1 (2.2.2.2)
</strong>      Origin IGP distance 20 metric 0, localpref 100, valid, external
      Community: 65004:200
      Last update: Sat Jul 11 07:40:50 2026

  Original VRF 0
  65003 65004
<strong>    172.18.13.1 from 172.18.13.1 (3.3.3.3)
</strong><strong>      Origin IGP distance 20 metric 0, localpref 200, valid, external, best
</strong>      Community: 65004:200
      Last update: Sat Jul 11 07:40:38 2026

</code></pre>

This time, both paths remain available. The communities identify the routes, while the inbound route-maps use local preference to select the preferred path.

### Exercise 3: Using a Well-Known Community

In this exercise, you will apply the well-known `no-export` community to one of the routes advertised by R4. R2 and R3 will receive the route, but they will not advertise it further to R1.

This demonstrates that some communities have a predefined meaning and do not require a route-map on the receiving router to apply their behavior.

```
config vdom
edit "R4"
    config router route-map
        edit "RM_SET_NO_EXPORT"
            config rule
                edit 100
                    set match-ip-address "PL_10.10.40.0"
                    set set-community "no-export"
                next
                edit 200
                next
            end
        next
    end
    config router bgp
        config neighbor
            edit "172.18.24.0"
                set route-map-out "RM_SET_NO_EXPORT"
            next
            edit "172.18.34.0"
                set route-map-out "RM_SET_NO_EXPORT"
            next
        end
    end
end
```

```
BGP-LAB (R4) # execute router clear bgp all soft out
```

Now let's verify what happens to 10.10.40.0 on each router.

<pre><code><strong>BGP-LAB (R4) # sudo R2 get router info bgp network 10.10.40.0/24
</strong>VRF 0 BGP routing table entry for 10.10.40.0/24
Paths: (1 available, best #1, table Default-IP-Routing-Table, not advertised to EBGP peer)
  Not advertised to any peer
  Original VRF 0
  65004
    172.18.24.1 from 172.18.24.1 (4.4.4.4)
      Origin IGP distance 20 metric 0, localpref 100, valid, external, best
<strong>      Community: no-export
</strong>      Last update: Sat Jul 11 07:56:02 2026



<strong>BGP-LAB (R4) # sudo R3 get router info bgp network 10.10.40.0/24
</strong>VRF 0 BGP routing table entry for 10.10.40.0/24
Paths: (1 available, best #1, table Default-IP-Routing-Table, not advertised to EBGP peer)
  Not advertised to any peer
  Original VRF 0
  65004
    172.18.34.1 from 172.18.34.1 (4.4.4.4)
      Origin IGP distance 20 metric 0, localpref 100, valid, external, best
<strong>      Community: no-export
</strong>      Last update: Sat Jul 11 07:56:21 2026



<strong>BGP-LAB (R4) # sudo R1 get router info bgp network 10.10.40.0/24
</strong><strong>% Network not in table
</strong>
</code></pre>

R2 and R3 both receive the no-export community and therefore no longer advertise the route to R1.\
Notice that neither R1,R2, nor R3 has a route-map to handle the `no-export` community. This behavior is built into BGP because `no-export` is a well-known community with a predefined meaning.

{% hint style="info" %}
**Other Well-Known Communities**

* `no-advertise`: Do not advertise the route to any BGP neighbor.
* `no-export`: Do not advertise the route outside the local AS.
* `no-export-subconfed`: Do not advertise the route outside the local confederation.
* `internet`: Advertise the route normally without export restrictions.
{% endhint %}

### Summary

In this lab, you learned how BGP communities can be used to tag routes and apply routing policies. You attached custom communities to routes on R4, verified that they were propagated through the network, and used route-maps on R1 to influence path selection based on those communities.

You also learned that route-maps have an implicit deny rule and that unmatched routes must be explicitly permitted if they should remain available as backup paths.

Finally, you used the well-known `no-export` community and observed how it automatically prevented routes from being advertised beyond the receiving autonomous system, without requiring any additional route-maps.

## Links

{% embed url="https://community.fortinet.com/fortigate-3/technical-tip-how-to-use-bgp-community-list-to-include-bgp-path-attributes-in-the-route-received-with-community-value-from-each-neighbor-96484" %}
