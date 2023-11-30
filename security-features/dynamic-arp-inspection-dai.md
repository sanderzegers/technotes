# Dynamic ARP inspection (DAI)

The primary goal of DAI is to protect the network against ARP Poisening and Man in the middle attacks.

ARP poisoning, also known as ARP spoofing, is a type of attack that involves sending falsified ARP (Address Resolution Protocol) messages to an Ethernet LAN. The purpose of these messages is to link the attacker's MAC address with the IP address of a legitimate computer or server on the network, which causes the traffic meant for that IP address to be misdirected to the attacker. This allows the attacker to intercept, modify, or stop data in transit, facilitating man-in-the-middle or denial-of-service attacks.

If DHCP Snooping is activated, two databases are maintained by the fortigate by monitoring DHCP Offers and Requests:

* DHCP snooping Client DB.
* IPSG (IP Source Guard) DB contains the statically assigned IPs

All ARP packets on untrusted ports without a valid entry in one of theses databases are dropped.

When an ARP Request or ARP reply arives on a untrusted port, it's checked against the IP, interface and VLAN. No DAI is happening on DHCP Snooping trusted ports.

DAI is is configured on a per-VLAN basis and can only be activated through the CLI:

```
config system interface
    edit "VLAN200"
        set vdom "root"
        set ip 10.1.5.1 255.255.255.0
        set allowaccess ping
        set switch-controller-dhcp-snooping enable
        set switch-controller-arp-inspection enable
        set interface "fortilink"
        set vlanid 200
    next
end
```

View DAI Statistics:

```
FG01# diagnose switch-controller switch-info arp-inspection stats S448EFTF2300001234:
Vdom: root

S448EFTF23007654:

vlan 200          arp-request               arp-reply         
-----------------------------------------------------------------------
received               11                      138            
forwarded              11                       4             
dropped                0                       134            
```

ARP inspection log entries:

<figure><img src="../.gitbook/assets/grafik (1).png" alt=""><figcaption></figcaption></figure>

Only malicious ARP Packets are dropped, the port remains functionial.

By default DAI is linked to the DHCP-snooping port setting, distinguishing between Trusted and Untrusted ports. ARP spoofing is permitted on Trusted ports, while it is blocked on Untrusted ports.\
However, it is possible to manually disable ARP inspection on a DHCP snooping Untrusted port.:

```
#config switch-controller managed-switch
  edit <switch-id>
     config ports
        edit port 3
           set dhcp-snooping untrusted
           set arp-inspection-trust trusted
        next
     end
  next
end
```
