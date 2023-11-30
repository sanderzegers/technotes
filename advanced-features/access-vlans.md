# Access VLANs

Access VLANs are designed to restrict connected devices within the VLAN to communicate exclusively with external devices or systems protected by a FortiGate firewall. Communication between devices within the same VLAN (intra-VLAN) is typically blocked, which can be advantageous in scenarios where clients require only internet access, without the need to interact with other devices on the local network. This configuration is commonly employed in environments such as guest networks, public access networks, or hotel Wi-Fi systems to enhance security and manage network resource access.

By implementing proxy ARP on the FortiGate, intra-VLAN device communication can be selectively permitted. This capability effectively allows the FortiGate to serve as an intermediary, facilitating controlled interactions between devices on the same VLAN. This strategy is a keystone of micro-segmentation, enhancing network security by allowing fine-grained control over the traffic flow within the VLAN.

For intra-VLAN traffic to be allowed through the FortiGate, appropriate firewall policies must be in place. These policies are necessary to ensure that the firewall examines all intra-VLAN communication, thus enabling the FortiGate to enforce network security policies and provide the requisite protections for the devices within the VLAN.



Access VLAN can be configured via the GUI in the VLAN interface settings:

<figure><img src="../.gitbook/assets/grafik (24).png" alt=""><figcaption></figcaption></figure>

Or via the CLI:

```
config system interface
    edit VLAN200
        set switch-controller-access-vlan enable
    next
end
```

Configure proxy ARP on the FortiGate to respond to ARP requests for all members within the VLAN. By doing so, you are instructing the FortiGate to act on behalf of all devices in the IP range, effectively replying to ARP inquiries directed at any IP address within the designated subnet. This ensures the FortiGate serves as the intermediary between both devices.

```
config system proxy-arp
    edit 1
        set interface "VLAN200"
        set ip 10.1.5.1
        set end-ip 10.1.5.254
    next
end
```

If the vlan interface is in a Zone, make sure to also block intra-zone traffic.

And configure to appropriate firewall rules:

<figure><img src="../.gitbook/assets/grafik (25).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
IPv6 is not supported for micro-segmentation, because it uses ICMPv6-ND to discover neighbours instead of ARP
{% endhint %}

{% hint style="info" %}
Intra-VLAN traffic is not supported when FortiLink interface type is hardware or software switch
{% endhint %}
