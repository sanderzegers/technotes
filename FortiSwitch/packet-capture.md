# Packet Capture

There are multiple variants to capture packets on the FortiSwitch. Not all methods can capture all type of packets. This might be confusing when capturing data for troubleshooting. See table below. The available methods are.

Becareful when combining different type of packet capture methods, often this is not supported.

* Diagnose Sniffer command
* Sniffer Profile
* SPAN (Switched Port Analyzer)
* RSPAN (Remote SPAN)
* ERSPAN (Encapsulated Remote SPAN)

{% hint style="danger" %}
RSPAN / ERSPAN is not supported on 1xxE and 1xxF Models
{% endhint %}

Overview:

<table data-full-width="true"><thead><tr><th>Method</th><th>Supported Capture Interfaces</th><th></th><th></th><th></th><th>Remarks</th><th>Traffic Type</th></tr></thead><tbody><tr><td></td><td>internal</td><td>mgmt</td><td>switchports</td><td>trunks</td><td></td><td></td></tr><tr><td>Sniffer Profile</td><td>x</td><td>x</td><td>x</td><td>(x)</td><td>Traffic in trunk may not include egress management traffic</td><td>No egress management traffic<br>Ingress management traffic<br>Regular Traffic</td></tr><tr><td></td><td></td><td></td><td></td><td></td><td></td><td></td></tr><tr><td>#diagnose sniffer</td><td>x</td><td>x</td><td>x</td><td></td><td>Only ingress Layer 2 Traffic is visible (LLDP and STP, etc) </td><td>No egress management traffic<br>No regular traffic<br>Only ingress management traffic</td></tr><tr><td>SPAN</td><td></td><td></td><td>x</td><td>x</td><td></td><td>No egress management traffic<br>No </td></tr><tr><td>RSPAN</td><td></td><td></td><td>x</td><td></td><td>Traffic always mirrored to Fortigate.</td><td>No egress management traffic<br>No </td></tr><tr><td></td><td></td><td></td><td></td><td></td><td>Does not capture Layer 2 traffic destinated or sourced from the FortiSwitcDoes not capture Layer 2 traffic destinated or sourced from the FortiSwitch</td><td></td></tr><tr><td>ERSPAN</td><td></td><td></td><td>x</td><td></td><td>Does not capture Layer 2 traffic destinated or sourced from the FortiSwitch</td><td>No egress management traffic<br>No </td></tr><tr><td>sFlow</td><td>x</td><td></td><td>x</td><td>x</td><td></td><td></td></tr><tr><td></td><td></td><td></td><td></td><td></td><td></td><td></td></tr></tbody></table>

\
Diagnose sniffer
----------------

CLI command on FortiSwitch:

\#diagnose sniffer packet \<interface> '\<filter>' \<level> \<count>

Same command syntax as Fortigate. Most commonly used levels are 4 and 6. You can convert level 6 output to pcap packets using the Fortigate tool: [https://community.fortinet.com/t5/FortiGate/Technical-Tip-How-to-import-diagnose-sniffer-packet-data-to/ta-p/191727](https://community.fortinet.com/t5/FortiGate/Technical-Tip-How-to-import-diagnose-sniffer-packet-data-to/ta-p/191727)

Only the internal and mgmt interface offer a 100% valid capture. Packet captures on the switchports only contain ingress layer 2 traffic suchs as LLDP and STP.

\
The notation for capturing switchports with the diagnose command is:  \_\_port\_\_21

```
FSW1# diagnose sniffer packet __port__21
FSW1# diagnose sniffer packet __port__21 none 6 10
```



## Sniffer Profile

This command ist best run via the FortiSwitch GUI. Via GUI it's possible to download the pcap file directly from the web interface. When running the commands in CLI, the pcap file must be transfered to a FTP or TFTP server.

Be careful when running sniffer profiles on Trunk interfaces. It may not include some egress traffic.

## SPAN

Mirror traffic ingress+egress from one switchport to another switchport. Both switchport must be on the same switch. SPAN traffic cannot traverse multiple switches. The  monitoring device must be connected to the same switch where traffic is being mirrored.

Sytem interfaces such as internal and mgmt cannot be captured. And it is not possible to define any filter options (Standalone switches can use ACL lists).

The mirrored port cannot be used for regular traffic. It will only forward all mirrored traffic to this port.

Fortigate Config

```
config switch-controller managed-switch
    edit S424DTPF123456
        config mirror
            edit "capture1"
                set status active
                set dst "port2"
                set src-ingress "port1"
                set src-egress "port1"
            next
        end
    next
end
```

Will result in following FortiSwitch config:

```
config switch mirror
    edit "capture1"
        set status active
        set dst "port2"
        set src-ingress "port1"
        set src-egress "port1"
    next
end
```



## RSPAN

Mirrors traffic from a switchport to a defined VLAN. Preferably you use a VLAN which is dedicated for this task. All devices configured for this VLAN, will receive the captured traffic.

On Managed FortiSwitch the switch uses a predefined RSPAN VLAN (VLAN ID 4092 /name:rspan). Traffic is forwarded accross the stack to port leading to the Fortigate only, thus preventing unnecessary traffic.&#x20;

An ACL can be configured on both standalone and managed switches to filter traffic.

System interfaces such as internal and mgmt cannot be captured.Only switchport can be captured. So no trunks or switchport members.

The configuration on the Fortigate is simplified a bit. You do not select a destination port and a capture vlan, it will be configured on the FortiSwitch according to your default VLAN Settings. To Mirror outgoing and incoming traffic on port1:

```
config switch-controller traffic-sniffer
    set mode rspan
    config target-port
        edit "S424DPTF12341234"
            set in-ports "port1"
            set out-ports "port1"
        next
    end
end
```

Resulting FortiSwitch config:

```
config switch mirror
    edit "flink.sniffer"
        set status active
        set mode RSPAN-auto
        set dst "port5"
        set src-ingress "port1"
        set src-egress "port1"
        set rspan-ip 10.255.1.1
        set encap-vlan-id 4092
    next
end
```

To see the forwarded traffic capture on the Fortigate:

```
diagnose sniffer packet rspan none 4 1000
```

In this example Native VLAN is VLAN 200. The ingress packets are shown as untagged in the catpure, the outgoing packets are encapsulated in the VLAN:

<figure><img src=".gitbook/assets/grafik (14).png" alt=""><figcaption></figcaption></figure>

If you run a packet capture In the GUI in FortiOS 7.2.5 on the rspan interface, it will only show the incoming packets, the tagged packets (outgoing) are not visible.



## ERSPAN

ERSPAN allows you to send capture accross a layer 3 connection with GRE encapsulation.\
It's possible to add an ACL to the sniffer, to further filter the captured traffic before forwarding.\
System interfaces are not supported and the destination cannot be in the FortiLink subnet.

Note: that it is not possible to use ERSPAN and SPAN mode captures simultaniously.

Fortigate Config:

```
config switch-controller traffic-sniffer
    set mode erspan-auto
    set erspan-ip 10.1.5.123
    config target-port
        edit "S424DPTF12341234"
            set in-ports "port1"
            set out-ports "port1"
        next
    end
end
```

Resulting FortiSwitch config:

```
config switch mirror
    edit "RSPAN"
            set status active
            set mote ERSPAN-auto
            set src-ingress "port1"
            set src-egress "port1"
            set erspan-collector-ip 10.1.5.123
    next
end
```

Next step is to create a firewall policy, which allows traffic from the RSPAN vlan to your collector station.

<figure><img src=".gitbook/assets/grafik (13).png" alt=""><figcaption></figcaption></figure>

On the collector station you should receive the packets defined by the filter.\
Note, that it does not capture STP,CDP and LLDP packets.



## sFlow

An alternative method for packet retrieval is through the use of sFlow. sFlow operates by randomly sampling packets and forwarding them to a specified IP address. By configuring the sampling rate to 1, it's possible to capture every packet from an interface, effectively retrieving all packets. Additionally, sFlow supports the capture of packets from trunk links, including Inter-Switch Link (ISL) and Inter-Chassis Link (ICL) configurations. FortiSwitch implements sFlow version 5.

Besides packet sampling, sFlowv5 is also capable of sending periodic interface counter data to the sFlow Collector. This process, known as counter sampling, provides valuable insights into network performance and utilization metrics

```
config switch-controller sflow
    set collector-ip 10.1.1.20
    set collector-port 200
end
```

Enable sFlow on a switch port interface:

```
config switch-controller managed-switch
    edit <switch-id>
        config ports
            edit <port>
                set packet-sampler enabled | disabled
                set packet-sample-rate 1
                set sflow-counter-interval 0
                set sample-direction both
```

In a default configuration, the packet sample rate is set to 512, meaning that the system forwards every 512th packet to the sFlow collector. It's set to 1 to capture every packet.

The sflow-counter-interval is disabled by default (0). It can send interface counters to an sFlow collector. Sampling direction is set to both, to capture ingress and egress packets.
