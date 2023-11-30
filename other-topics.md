# Other Topics

## Fabric lock down

Make the automatic generated Trunks between the FortiSwitches static, so they will not be automatically removed after the timeout expires:&#x20;

By default ISL trunks will automatically get removed if the physical link is up and no LLDP or FortiLink packets are received within 60s (TODO: Verify timeout).

You can configure a ISL or ICL trunk to become static, by using the 'set static-isl enable' parameter.\
Fabric-lockdown can do this automatically on all existing trunks.

```
FortiGate-60E # diagnose switch-controller switch-recommendation fabric-lockdown-check fortilink
ret(34)
Output message : FortiSwitch fabric is not locked down; recommend to lock down the fabric
```

```
FortiGate-60E # diagnose switch-controller switch-recommendation fabric-lockdown-enable fortilink
ret(0)
Output message : Successful operation.
```

'set static-isl enable'

```
config switch trunk
    edit "_FlInK1_ICL0_"
        set mode lacp-active
        set auto-isl 1
        set mclag-icl enable
        set static-isl enable
        set members "port47" "port48"         
    next
    edit "8EFTF23007654-0"
        set mode lacp-active
        set auto-isl 1
        set mclag enable
        set static-isl enable
        set members "port25"         
    next
    edit "GT60E4Q16046841"
        set mode lacp-active
        set auto-isl 1
        set fortilink 1
        set mclag enable
        set members "port46"         
    next
end

```



## VLAN optimization

VLAN optimization is enabled by default. It will only allow existing VLANs on all ISL/ICL links:

```
    edit "_FlInK1_ICL0_"
        set native-vlan 4094
        set allowed-vlans 1,200,4088-4094
        set dhcp-snooping trusted
        set edge-port disabled
        set igmp-snooping-flood-reports enable
        set mcast-snooping-flood-traffic enable
        set snmp-index 62
    next
    edit "GT60E4Q16046841"
        set native-vlan 4094
        set allowed-vlans 1,200,4088-4094
        set dhcp-snooping trusted
        set stp-state disabled
        set snmp-index 61
    next
```

By disabling vlan-optimization, it will allow all VLANs to pass traffic over the trunks. Also for the unused VLANs.

```
config switch-controller global
    set vlan-optimization disable
end
```

```
    edit "_FlInK1_ICL0_"
        set native-vlan 4094
        set allowed-vlans 1-4094
        set dhcp-snooping trusted
        set edge-port disabled
        set igmp-snooping-flood-reports enable
        set mcast-snooping-flood-traffic enable
        set snmp-index 62
    next
    edit "GT60E4Q16046841"
        set native-vlan 4094
        set allowed-vlans 1-4094
        set dhcp-snooping trusted
        set stp-state disabled
        set snmp-index 61
    next
```

VLAN optimization must be disabled if you want use vlan allowed all mode. See next chapter.

## VLAN-All mode

{% embed url="https://docs.fortinet.com/document/fortiswitch/6.4.6/administration-guide/146333/vlans-and-vlan-tagging" %}

## MAC Aging

By default mac address timeout after 5min. This is a good default value. Optionally this can be changed:

```
config switch-controller global
    set mac-aging-interval <seconds>
end
```



## Power over Ethernet (PoE)

Some power delivered to powered device (PD) is dissipated on the cable. That why there is a difference between max power on port and guaranteed power.

| Name | IEEE Standard  | Max Power on port | Guaranteed power on PD |
| ---- | -------------- | ----------------- | ---------------------- |
| PoE  | 802.3af        | 15.4W             | 12.95W                 |
| PoE+ | 802.3at        | 30W               | 25.50W                 |
| UPoE | 802.3bt type 3 | 60W               | 51W                    |

PoE and FPOE switches have a certain power budget. Although a FPOE switch could deliver power on all ports on the switch. It cannot deliver it to all ports at the same time at maximum power.

When power limit has reached, it will shutdown ports according to a pre-defined logic. To logics are available: Priority based or First Come, first serve. On a standalone switch you configure this parameter:

```
config switch global
    set poe-power-mode priority|first-come-first-served
end
```

On managed switch 'priority' mode is default and the only available.

\
You can assign PoE priorities to ports. Depending on the Switch model you have four or three PoE priorities:

critical, high, (medium), low

To power priority can be assigned in the switch port configuration:

```
config switch-controller managed-switch
    edit "S448EFTF23007648"
        config ports         
            edit "port2"
                set status down
                set poe-capable 1
                set poe-port-priority high-priority
            next
        end
    next
end
```

If power limit has reached, it will cut off power to ports with lower priority. If ports have the same priority, it will shut the down the port with the lowest port number.&#x20;

Power measurement is base on real power usage of the device.

If a high priority port is connected, it will power on the device immediately. If power limit is reach, it wil thenl turn of a low priority device.

If a low priority device is connected, it will only power on the device if power budget + guard band has enough power.



Do not connect to PoE port to each other. If you absolutely have to, disable PoE on the switchport one side.
