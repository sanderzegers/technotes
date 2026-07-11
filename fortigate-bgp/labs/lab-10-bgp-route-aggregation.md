# Lab 10: BGP Route Aggregation

## Objective

In this lab, you will configure BGP route aggregation on a FortiGate. You will advertise multiple specific networks, create a summary route, and compare the routes before and after aggregation. You will also use `summary-only` to suppress the more-specific routes and verify the result in the BGP and routing tables.

## Topology

<figure><img src="https://github.com/sanderzegers/technotes/raw/refs/heads/undefined/fortigate-bgp/assets/topologies/exports/bgp-lab-master-Lab10.svg" alt=""><figcaption></figcaption></figure>

<table><thead><tr><th width="111.4166259765625">Router</th><th width="95.8333740234375" align="right">AS</th><th width="182.75">Interface IP</th><th width="201.5">BGP Neighbor</th><th>Local Networks</th></tr></thead><tbody><tr><td>R1</td><td align="right">65001</td><td>R1-R2: <code>172.18.12.0/31</code><br>R1-R3: <code>172.18.13.0/31</code></td><td>R2: <code>172.18.12.1</code><br>R3: <code>172.18.13.1</code></td><td>-</td></tr><tr><td>R2</td><td align="right">65002</td><td>R1-R2: <code>172.18.12.1/31</code><br>R2-R4: <code>172.18.24.0/31</code></td><td>R1: <code>172.18.12.0</code><br>R4: <code>172.18.24.1</code></td><td>-</td></tr><tr><td>R3</td><td align="right">65003</td><td>R1-R3: <code>172.18.13.1/31</code><br>R3-R4: <code>172.18.34.0/31</code></td><td>R1: <code>172.18.13.0</code><br>R4: <code>172.18.34.1</code></td><td>-</td></tr><tr><td>R4</td><td align="right">65004</td><td>R2-R4: <code>172.18.24.1/31</code><br>R3-R4: <code>172.18.34.1/31</code></td><td>R2: <code>172.18.24.0</code><br>R3: <code>172.18.34.0</code></td><td><p>R4-LAN0:<br><code>10.10.4.0/24</code></p><p>R4-LAN1:</p><p><code>10.10.40.0/24</code><br>R4-LAN2:</p><p><code>10.10.41.0/24</code><br>R4-LAN3:</p><p><code>10.10.42.0/24</code><br>R4-LAN4:</p><p><code>10.10.43.0/24</code></p></td></tr></tbody></table>

## Packet Captures

<table><thead><tr><th width="174">PCAP File</th><th>Description</th><th>What to look for</th></tr></thead><tbody><tr><td><a href="https://github.com/sanderzegers/technotes/raw/refs/heads/undefined/fortigate-bgp/pcaps/Lab10/lab10-simple-route-aggregation.pcapng">lab10-simple-route-aggregation.pcapng</a></td><td>Exercise 1: 10.10.40.0/22 route aggregation configured</td><td>BGP UPDATE contains two new Path attributes: <code>ATOMIC_AGGREGATE</code> and <code>AGGREGATOR</code><br><br><code>ATOMIC_AGGREGATE</code> indicates that the route was created by aggregating more-specific routes<br><br><code>AGGREGATOR</code> identifies AS 65004 and router ID 4.4.4.4 as the router that created the aggregate.</td></tr></tbody></table>

## BGP Route Aggregation

### Baseline configuration

Let's reset the lab, and configure the same baseline as the previous lab 9. This time we add additional local LANs which we are going to summarize later.

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
            edit R4-LAN2
                set vdom "R4"
                set ip 10.10.41.1 255.255.255.0
                set allowaccess ping
                set type loopback
            next
            edit R4-LAN3
                set vdom "R4"
                set ip 10.10.42.1 255.255.255.0
                set allowaccess ping
                set type loopback
            next
            edit R4-LAN4
                set vdom "R4"
                set ip 10.10.43.1 255.255.255.0
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
                    set prefix 10.10.40.0 255.255.255.0
                next
                edit 2
                    set prefix 10.10.41.0 255.255.255.0
                next
                edit 3
                    set prefix 10.10.42.0 255.255.255.0
                next
                edit 4
                    set prefix 10.10.43.0 255.255.255.0
                next
            end
        end
    next
end
```

Verify the configuration by looking at the received routes on R1:

```
BGP-LAB (R1) # get  router info bgp network
VRF 0 BGP table version is 1, local router ID is 1.1.1.1
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              S Stale
Origin codes: i - IGP, e - EGP, ? - incomplete

   Network          Next Hop            Metric     LocPrf Weight RouteTag Path
*  10.10.40.0/24    172.18.12.1     0                      0        0 65002 65004 i <-/->
*>                  172.18.13.1     0                      0        0 65003 65004 i <-/1>
*  10.10.41.0/24    172.18.12.1     0                      0        0 65002 65004 i <-/->
*>                  172.18.13.1     0                      0        0 65003 65004 i <-/1>
*  10.10.42.0/24    172.18.12.1     0                      0        0 65002 65004 i <-/->
*>                  172.18.13.1     0                      0        0 65003 65004 i <-/1>
*  10.10.43.0/24    172.18.12.1     0                      0        0 65002 65004 i <-/->
*>                  172.18.13.1     0                      0        0 65003 65004 i <-/1>

Total number of prefixes 4

BGP-LAB (R1) # get  router info routing-table bgp
Routing table for VRF=0
B       10.10.40.0/24 [20/0] via 172.18.13.1 (recursive is directly connected, R1R3-1-0), 00:01:35, [1/0]
B       10.10.41.0/24 [20/0] via 172.18.13.1 (recursive is directly connected, R1R3-1-0), 00:01:35, [1/0]
B       10.10.42.0/24 [20/0] via 172.18.13.1 (recursive is directly connected, R1R3-1-0), 00:01:35, [1/0]
B       10.10.43.0/24 [20/0] via 172.18.13.1 (recursive is directly connected, R1R3-1-0), 00:01:35, [1/0]
```

### Exercise 1: Assigning BGP Communities

Let's configure the route summarization on R4:

```
config vdom
    edit R4
        config router bgp
            config aggregate-address 
                edit 1 
                    set prefix 10.10.40.0/22
                next
            end
        end
    next
end
```

Now verify the routes on R4 and R1:

<pre><code>BGP-LAB (R4) # get router info bgp network 
VRF 0 BGP table version is 2, local router ID is 4.4.4.4
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              S Stale
Origin codes: i - IGP, e - EGP, ? - incomplete

   Network          Next Hop            Metric     LocPrf Weight RouteTag Path
<strong>*> 10.10.40.0/22    0.0.0.0                            32768        0 i &#x3C;-/1>
</strong>*> 10.10.40.0/24    0.0.0.0                       100  32768        0 i &#x3C;-/1>
*> 10.10.41.0/24    0.0.0.0                       100  32768        0 i &#x3C;-/1>
*> 10.10.42.0/24    0.0.0.0                       100  32768        0 i &#x3C;-/1>
*> 10.10.43.0/24    0.0.0.0                       100  32768        0 i &#x3C;-/1>

Total number of prefixes 5


BGP-LAB (R4) # sudo R1 get router info bgp network
VRF 0 BGP table version is 2, local router ID is 1.1.1.1
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              S Stale
Origin codes: i - IGP, e - EGP, ? - incomplete

   Network          Next Hop            Metric     LocPrf Weight RouteTag Path
<strong>*  10.10.40.0/22    172.18.13.1     0                      0        0 65003 65004 i &#x3C;-/->
</strong>*>                  172.18.12.1     0                      0        0 65002 65004 i &#x3C;-/1>
*  10.10.40.0/24    172.18.12.1     0                      0        0 65002 65004 i &#x3C;-/->
*>                  172.18.13.1     0                      0        0 65003 65004 i &#x3C;-/1>
*  10.10.41.0/24    172.18.12.1     0                      0        0 65002 65004 i &#x3C;-/->
*>                  172.18.13.1     0                      0        0 65003 65004 i &#x3C;-/1>
*  10.10.42.0/24    172.18.12.1     0                      0        0 65002 65004 i &#x3C;-/->
*>                  172.18.13.1     0                      0        0 65003 65004 i &#x3C;-/1>
*  10.10.43.0/24    172.18.12.1     0                      0        0 65002 65004 i &#x3C;-/->
*>                  172.18.13.1     0                      0        0 65003 65004 i &#x3C;-/1>

Total number of prefixes 5

BGP-LAB (R4) # sudo R1 get router info bgp network 10.10.40.0/22
VRF 0 BGP routing table entry for 10.10.40.0/22
Paths: (2 available, best #2, table Default-IP-Routing-Table)
  Advertised to non peer-group peers:
   172.18.12.1
  Original VRF 0
<strong>  65002 65004, (aggregated by 65004 4.4.4.4)
</strong>    172.18.12.1 from 172.18.12.1 (2.2.2.2)
<strong>      Origin IGP distance 20 metric 0, localpref 100, valid, external, atomic-aggregate
</strong>      Last update: Sat Jul 11 12:56:21 2026

  Original VRF 0
<strong>  65003 65004, (aggregated by 65004 4.4.4.4)
</strong>    172.18.13.1 from 172.18.13.1 (3.3.3.3)
<strong>      Origin IGP distance 20 metric 0, localpref 100, valid, external, atomic-aggregate, best
</strong>      Last update: Sat Jul 11 12:55:54 2026

</code></pre>

The new summarized /22 route is announced correctly. We can see on R1, that R4 summarized this route.

The old /24 are also still being announced. Let's stop the announcement.

<pre><code>config vdom
    edit "R4"
    config router bgp
        set as 65004
        set router-id 4.4.4.4
        config aggregate-address
            edit 1
                set prefix 10.10.40.0 255.255.252.0
<strong>                set summary-only enable
</strong>            next
        end
    end
end
</code></pre>

And verify again on R1 and R4.

Note that the /24 are still in the BGP table on R4. But they are not being announced anymore:

<pre><code>BGP-LAB (R4) # sudo R4 get router info bgp neighbors 172.18.24.0 advertised-routes
VRF 0 BGP table version is 5, local router ID is 4.4.4.4
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal
Origin codes: i - IGP, e - EGP, ? - incomplete

   Network          Next Hop            Metric     LocPrf Weight RouteTag Path
<strong>*> 10.10.40.0/22    172.18.24.1                        32768        0 i &#x3C;-/->
</strong>
Total number of prefixes 1


BGP-LAB (R4) # 
BGP-LAB (R4) # sudo R4 get router info bgp network 
VRF 0 BGP table version is 5, local router ID is 4.4.4.4
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              S Stale
Origin codes: i - IGP, e - EGP, ? - incomplete

   Network          Next Hop            Metric     LocPrf Weight RouteTag Path
<strong>*> 10.10.40.0/22    0.0.0.0                            32768        0 i &#x3C;-/1>
</strong><strong>s> 10.10.40.0/24    0.0.0.0                       100  32768        0 i &#x3C;-/1>
</strong><strong>s> 10.10.41.0/24    0.0.0.0                       100  32768        0 i &#x3C;-/1>
</strong><strong>s> 10.10.42.0/24    0.0.0.0                       100  32768        0 i &#x3C;-/1>
</strong><strong>s> 10.10.43.0/24    0.0.0.0                       100  32768        0 i &#x3C;-/1>
</strong>
Total number of prefixes 5
</code></pre>

<pre><code>BGP-LAB (R4) # sudo R1 get router info bgp network 10.10.42.0/24
% Network not in table

BGP-LAB (R4) # sudo R1 get router info bgp network 10.10.40.0/22
<strong>VRF 0 BGP routing table entry for 10.10.40.0/22
</strong>Paths: (2 available, best #2, table Default-IP-Routing-Table)
  Advertised to non peer-group peers:
   172.18.12.1
  Original VRF 0
  65002 65004, (aggregated by 65004 4.4.4.4)
    172.18.12.1 from 172.18.12.1 (2.2.2.2)
      Origin IGP distance 20 metric 0, localpref 100, valid, external, atomic-aggregate
      Last update: Sat Jul 11 12:56:21 2026

  Original VRF 0
<strong>  65003 65004, (aggregated by 65004 4.4.4.4)
</strong>    172.18.13.1 from 172.18.13.1 (3.3.3.3)
      Origin IGP distance 20 metric 0, localpref 100, valid, external, atomic-aggregate, best
      Last update: Sat Jul 11 12:55:54 2026


BGP-LAB (R4) # sudo R1 get router info routing-table bgp
Routing table for VRF=0
<strong>B       10.10.40.0/22 [20/0] via 172.18.13.1 (recursive is directly connected, R1R3-1-0), 00:11:30, [1/0]
</strong>
</code></pre>

### Exercise 3: Test aggregate Route Dependency

Disable all but one interface in the 10.10.40.0/22 range. Then observe what happens to the advertised route summary:

```
config vdom
edit "R4"
    config system interface
        edit R4-LAN2
            set status down
        next
        edit R4-LAN3
            set status down
        next
        edit R4-LAN4
            set status down
        next
    end
end
```

Verify that only the R4-LAN1 network is still connected:

<pre><code>BGP-LAB (R4) # get router info routing-table connected 
Routing table for VRF=0
<strong>C       10.10.4.0/24 is directly connected, R4-LAN0
</strong>C       10.10.40.0/24 is directly connected, R4-LAN1
C       172.17.0.4/32 is directly connected, R4-LO0
C       172.18.14.0/31 is directly connected, R1R4-1
C       172.18.24.0/31 is directly connected, R2R4-1
C       172.18.34.0/31 is directly connected, R3R4-1
C       172.19.0.0/24 is directly connected, R4NET1-0
</code></pre>

Check BGP announcements:

<pre><code>BGP-LAB (R4) # get router info bgp neighbors 172.18.24.0 advertised-routes 
VRF 0 BGP table version is 6, local router ID is 4.4.4.4
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal
Origin codes: i - IGP, e - EGP, ? - incomplete

   Network          Next Hop            Metric     LocPrf Weight RouteTag Path
<strong>*> 10.10.40.0/22    172.18.24.1                        32768        0 i &#x3C;-/->
</strong>
Total number of prefixes 1

BGP-LAB (R4) # sudo R1 get router info routing-table bgp
Routing table for VRF=0
<strong>B       10.10.40.0/22 [20/0] via 172.18.13.1 (recursive is directly connected, R1R3-1-0), 00:17:11, [1/0]
</strong>

</code></pre>

The `/22` route remains advertised, although three interfaces are down. The associated networks are unreachable.

Disable the final interface in this range: `R4-LAN1`.

```
config vdom
edit "R4"
    config system interface
        edit R4-LAN1
            set status down
        next
    end
end
```

And verify again:

```
BGP-LAB (R4) # get router info bgp neighbors 172.18.24.0 advertised-routes 
% No prefix for neighbor 172.18.24.0

BGP-LAB (R4) # sudo R1 get router info routing-table bgp
No route available

```

The summary route is not announced anymore. To announce a summary route, at least one interface in the summary range must be online.

## Summary

In this lab, you configured BGP route aggregation on a FortiGate. R4 combined four `/24` networks into the single `10.10.40.0/22` aggregate route.

You verified that BGP advertises both the aggregate and the specific routes by default. You then enabled `summary-only` to suppress the more-specific routes from BGP updates.

You also examined the `ATOMIC_AGGREGATE` and `AGGREGATOR` path attributes, verified advertised routes with FortiGate commands, and observed the withdrawal of the specific routes in a packet capture.

Finally, you tested how the aggregate depends on its component routes and discussed the risk of advertising address space for which no valid specific route exists.

## Links

{% embed url="https://community.fortinet.com/fortigate-3/technical-tip-how-to-implement-bgp-route-summary-aggregation-on-a-fortigate-97875" %}
