# IGMP Snooping

## IGMP Protocol

#### 1. **Messages:**

* **Membership Query:** Sent by routers to discover which hosts belong to a multicast group.
* **Membership Report:** Sent by hosts to announce that they belong to a multicast group.
* **Leave Group:** Introduced in IGMPv2, this message is sent by a host to signal that it intends to leave a multicast group.

#### 2. **Host Membership States:**

* **Non-Member:** The host does not belong to any multicast group.
* **Delaying Member:** The host belongs to a group but is waiting for a random time before sending a report.
* **Idle Member:** The host belongs to a group and has heard a report from another member.

#### 3. **Router Behavior:**

* **General Query:** Sent periodically by the router to discover which hosts belong to a multicast group.
* **Group-Specific Query:** Sent by the router in response to a Leave Group message to check if there are remaining members of the group on the network.

#### 4. **Host Behavior:**

* **Joining a Group:** A host sends a Membership Report to join a multicast group.
* **Leaving a Group:** A host sends a Leave Group message when it wants to leave a group. The router then sends a Group-Specific Query to check for remaining members.

#### 5. **Group Addresses:**

* IGMPv2 uses special multicast IP addresses to communicate. For example, the address `224.0.0.1` is the all-hosts group, and `224.0.0.2` is the all-routers group.



## IGMP Features

By default FortiSwitch handles multicast traffic in the same way as broadcast. Multicast frames are flooded to all switches and switchports. This can lead to performance issues as well as data leaks.

If a client wants to receive multicast traffic, it should send a IGMP report join message. IGMP snooping listens to IGMP multicast messages. It only forwards the specific multicast stream traffic to these clients. And it stops sending if a client leaves the multicast group.

FortiSwitch supports IGMP v1 and v2 fully. For Version 3 it doesn't support source filtering, which is a feature that enables to limit traffic to a specific soure address that receiver wants to get packet from. This filter rules is ignored, and traffic is handled like an IGMPv2 request.



If IMGP is enabled, FortiSwitch maintains a Multicast Layer 2 Forwarding Table. It will monitor IGMP report join and leave message and will update the table accorcdling, only ports with receivers will receive the multicast traffic.

IGMP report messages are not forwarded, except to to mRouter ports. Normally the clients only sends one IGMP report join message, they do not send additionial join messages except if they are asked to do so caused by receiving a IGMP report join message.

By default only recievers can receive multicast traffic from senders on the same switch.

To solve both problems, you can enable IGMP snooping querier on the FortiSwitch. The Switch will send periodic IGMP query messages, to get IGMP membership reports from receivers.



IGMP Configuration

Enabling IGMP snooping is simple. Either through the GUI in the VLAN Setting, or CLI:

Wifi & Switch Controller -> FortiSwitch VLANs -> Select VLAN

![](<../.gitbook/assets/grafik (21).png>)

```
config system interface
    edit "VLAN200"
        set switch-controller-igmp-snooping enable
```

FortiSwitches will now monitor all IGMP Report messages, and enable forwarding on member ports.\
Some applications do not send and a IGMP report message, or the FortiSwitch might have missed it. It's recommended to let the FortiSwitch send a Membership Report message regulary. This will ask all members, to report their membership.

Another workaround for this issue, is to flood-unknown-multicase to all ports:

```
config switch-controller igmp-snooping
    set flood-unknown-multicase enable
```

It's sufficient to configure one querier on a core FortiSwitch, all other Switches will monitor and apply the report messages. Querier-addr should be set to Gateway address.

```
config switch-controller managed-switch
    edit "S448EFTF12345" # Coreswitch
        config igmp-snooping
            set local-override enable
            set aging-time 300
            config vlans
                edit "VLAN200"
                    set querier enable
                    set querier-addr 10.1.5.1
                    set version 2
                next
            end
        end
    next
end
```

Aging-time is 300sec default and should work for most setups.

Two additional tuning options are igmp-snooping proxy and fast-leave.\
FortiSwitch forwards IGMP reports to all mRouter ports, this can cause heave load on the IGMP querier in large networks with a very high number of multicast receivers. With igmp-snooping-proxy, IGMP reports are only forwarded when the first member of a multicast group joins or its last member leaves.

By default the FortiSwitch in igmp-snooping-proxy mode continues to forward multicast traffic for 10sec after the receiver left the group. This option will stop forwarding multicast traffic immediately.

```
config system interface
    edit "VLAN200"
        set switch-controller-igmp-snooping enable
        set switch-controller-igmp-snooping-proxy enable
        set switch-controller-igmp-snooping-fast-leave enable
```

To monitor the multicast status:

```
FG60E # diagnose switch-controller switch-info igmp-snooping group S448EFTF23007146 

S448EFTF23007146:

IGMP-SNOOPING learned mcast-groups:
port             VLAN    GROUP                           Age-timeout      IGMP-Version
_FlInK1_ICL0_    ---     flood-reports                   --               --
_FlInK1_ICL0_    ---     flood-traffic                   --               --
8EFTF23007654-0  200     239.255.255.250                 265              V2
8EFTF23007654-0  200     224.2.2.1                       272              V2
_FlInK1_ICL0_    200     239.255.255.250                 266              V2
_FlInK1_ICL0_    200     224.2.2.1                       272              V2
port13           200     224.2.2.1                       266              V2
port14           200     224.2.2.1                       274              V2

Total IGMP Hosts: 6

----

FG60E # diagnose switch-controller switch-info igmp-snooping status S448EFTF23007146 

S448EFTF23007146:
IGMP-SNOOPING enabled vlans:

VLAN     PROXY            QUERIER VERSION
----     -----            ---------------
200      DISABLED         V2


Max IGMP snooping groups 1023

Total IGMP groups 2 (Static 0)
Remaining allowed IGMP snooping groups: 1021
```
