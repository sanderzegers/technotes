# Lab 6: Prefix-lists and route filtering

## Objective

In this lab we learn how to control BGP route advertisements. We start by advertising multiple networks, then use prefix-lists to allow or block specific routes. Finally, we introduce route-maps as a more flexible way to match and control BGP routes.

## Topology

<figure><img src="https://github.com/sanderzegers/technotes/raw/refs/heads/undefined/fortigate-bgp/assets/topologies/exports/bgp-lab-master-Lab1.svg" alt=""><figcaption></figcaption></figure>

| Router |    AS | Interface IP | BGP Neighbor | Local Networks                       |
| ------ | ----: | ------------ | ------------ | ------------------------------------ |
| R1     | 65002 | 172.18.12.0  | 172.18.12.1  | <p>10.10.1.0/24<br>10.10.11.0/24</p> |
| R2     | 65003 | 172.18.12.1  | 172.18.12.0  | 10.10.2.1/24                         |

## Packet Captures

<table><thead><tr><th width="174">PCAP File</th><th>Description</th><th>What to look for</th></tr></thead><tbody><tr><td></td><td></td><td></td></tr></tbody></table>

## Outbound prefix-list

We'll start with the eBGP peering from Lab2. Re-Load the lab baseline and execute:

We'll add an additional loopback interface to R1

```
config vdom
edit R1
config system interface
    edit "R1-LAN1"
        set vdom "R1"
        set ip 10.10.11.1 255.255.255.0
        set allowaccess ping
        set type loopback
    next
end
end
```

Next we'll configure the peering between R1 and R2 and announce both LAN IP Ranges

```
config vdom
edit R1
config router bgp
    set as 65002
    config neighbor
        edit "172.18.12.1"
            set remote-as 65003
        next
    end
      
end
next
edit R2
config router bgp
    set as 65003
    config neighbor
        edit "172.18.12.0"
            set remote-as 65002
        next
    end
end
```

Verify that R2 received both routes:

```
BGP-LAB (R2) # get router info bgp sum

VRF 0 BGP router identifier 172.17.0.2, local AS number 65003
BGP table version is 1
2 BGP AS-PATH entries
0 BGP community entries

Neighbor    V         AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
172.18.12.0 4      65002       6       4        0    0    0 00:02:11        2

Total number of neighbors 1


BGP-LAB (R2) # get router info routing-table bgp
Routing table for VRF=0
B       10.10.1.0/24 [20/0] via 172.18.12.0 (recursive is directly connected, R1R2-1), 00:01:05, [1/0]
B       10.10.11.0/24 [20/0] via 172.18.12.0 (recursive is directly connected, R1R2-1), 00:00:36, [1/0]

```

## Prefix-list, access-list and route-map

| Object          | Main job                     | Simple explanation                                 |
| --------------- | ---------------------------- | -------------------------------------------------- |
| **Prefix-list** | Match route prefixes         | “Which networks do I mean?”                        |
| **Access-list** | Match route prefixes         | Older/simple way to match networks                 |
| **Route-map**   | Match routes and take action | “If this route matches, what should I do with it?” |

### Prefix-list

The Prefix-list is used to match network prefixes. This is usually the cleanest option for BGP route filtering.

You can see a simple prefix-list example below. Note The default action for any non specified prefix is deny.

```
config router prefix-list    
    edit "route-in"
        config rule
            edit 1
                set action permit
                set prefix 10.10.1.0 255.255.255.0
            next
        end
    end
end
```

Optionally the prefix ranges can be broaden by using the `ge` and `le` parameters.

```
config router prefix-list
    edit "route-in"
        config rule
            edit 1
                set action permit
                set prefix 10.10.1.0 255.255.255.0
                set ge 25
                set le 27
            next
        end
    end
end
```

This needs some additional explanation.&#x20;

`ge` stands for greater or equals.\
In the example below, the subnet mask is set for the prefix is set to /24.

`ge 25` means prefix length **greater than or equal to 25**.\
&#x20;/25, /26, /27, /28, /29, /l30, /31 , /32

`le 27` means prefix length **less than or equal to 27**.\
&#x20;/27, /26, /25, /24

Combined together this means /25, /26, /27.

So this prefix-list allows subnets inside `10.10.1.0/24` with a prefix length from `/25` to `/27`.

Some examples:

```
10.10.1.0/25
10.10.1.128/25
10.10.1.0/26
10.10.1.64/26
10.10.1.0/27
10.10.1.32/27
```

### Access-lists



## Links

{% embed url="https://community.fortinet.com/fortigate-3/technical-tip-how-to-control-bgp-route-advertisement-with-prefix-list-95213" %}

{% embed url="https://community.fortinet.com/fortigate-3/technical-tip-how-to-combine-operators-ge-and-le-in-prefix-list-for-route-map-for-filtering-bgp-routes-104051" %}
