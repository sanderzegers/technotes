# Root Guard

A change of the root switch in a spanning-tree topology, can cause a network disruption and reroute of traffic. To prevent this from happening root guard was introduced.

When root guard is enabled on a port, it ignores superior BPDUs and thus prevents root bridge change.

If a superioir BPDU is detected, it will not accept the new root bridge and the port will go into alternative role, thus not forwarding any traffic.\
If a BPDU with a lower bridge id is detected, the switch will participate normally in the spanning-tree topology.

Following log entries are created:

<figure><img src="../.gitbook/assets/grafik (10).png" alt=""><figcaption></figcaption></figure>

As soons as the other bridge priority goes down, the port changes back to forwarding mode:

<figure><img src="../.gitbook/assets/grafik (11).png" alt=""><figcaption></figcaption></figure>

Root guard is also visible on the switch, when the 'RG' flag is active on a port :

```
#diagnose stp instance list
--- cut ---

  CISCO_SW1          100M    200000     128        ALTERNATIVE  DISCARDING   2          EN RG 
  CISCO_SW2          100M    200000     128        ALTERNATIVE  DISCARDING   2          EN RG 
  SW1                2G      1          128        ROOT         FORWARDING   2          EN 

  Flags: EN(STP enable), ED(Edge), LP(Loop Protection Triggered)
  RG(Root Guard Triggered), BG(BPDU Guard Triggered), IC(PVST Port Inconsistent)
  MV(PVST Port Vlan Mismatch)

--- cut ---
```

Deploy root guard on ports where you do not expect to see any BPDUs from a switch that could attempt to become the root bridge.

Typically used on ports leading toward access layers or edge parts of the network where you want to ensure the core or distribution layer remains the STP root.

At its core, Root Guard serves to protect the status of the existing root bridge, ensuring that the structure of the STP topology remains intact against unforeseen changes from new root bridges.&#x20;

On the other hand, BPDU Guard primarily focuses on the overall integrity of the network. It does this by swiftly disabling any ports that detect the presence of unexpected devices, particularly those that might be transmitting BPDUs

