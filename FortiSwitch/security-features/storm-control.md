# Storm Control

Storm Control limits the rates of unknown unicast packets, broadcast packets and/or unknown multicast packets.

By default, this limit is set to 500 packets/second, but it's disabled by default. If the traffic rate for any of the types exceeds the configured threshold, the FortiSwitch unit drops the excess traffic. Packets above this threshold will be dropped.

Only packets categorized by the traffic types are blocked. All other packets are forwarded regularly on the switch port.

By default, storm control is disabled on the mclag-icl, isl and fortilink connections. It's possible to define a global storm control policy, or a per-port storm control policy. The preferred way is to enable storm-control only on client edge ports. The "Edge-Port" storm-control-policy is already assigned to all access ports by default. The default setting of the Edge-Port storm-control policy is to use the global storm-control settings.

Changing the global storm-control-policy will enable storm-control on all non-ICL, ISL or FortiLink ports.

```
config switch-controller storm-control
    set rate 500
    set burst-size-level <0-4>
    set unknown-unicast enable
    set unknown-multicast enable
    set broadcast enable
end
```

Storm-control is implemented in hardware, so there are no logs to indicate traffic dropped by storm-control.&#x20;

To verify it's working add following hw-counter. Add a PDISC hardware counter:

`FSW# diagnose switch phyiscal-ports hw-counter add rx 4 PDISC port2`

To check the counters:&#x20;

`FSW# diagnose switch physical-ports hw-counter show rx port2`

The PDISC counter will show the dropped packets.

```
SW6 # diagnose switch physical-ports hw-counter show rx port1
-------------------------------------------------------------------------------------
|                              Counter Statistics (port:port1)                        
-------------------------------------------------------------------------------------
|Type|Counter ID|       Value        |           Trigger Flags Enabled     
-------------------------------------------------------------------------------------
| Rx |         0|              132683|RIPD4 RIPD6 RDISC RPORTD PDISC     
|    |          |                    | RFILDR RDROP VLANDR               
-------------------------------------------------------------------------------------
| Rx |         1|              112976|IMBP                               
-------------------------------------------------------------------------------------
| Rx |         2|                   0|RIMDR                              
-------------------------------------------------------------------------------------
| Rx |         4|               12903|PDISC                              
-------------------------------------------------------------------------------------
```

Since FortiSwitchOS 7.4.3 you can also use `storm-control-monitor` to generate log message when a specific threshold is exceeded and get a better overview:

{% embed url="https://docs.fortinet.com/document/fortiswitch/8.0.0/fortiswitchos-administration-guide/13233/storm-control#Monitorn" %}

### Storm control types

**unknown-unicast**: Unicast destination MAC is not in MAC table\
**unknown-multicast**: Multicast destination MAC is not in MAC table\
**broadcast**: Broadcast packets

{% embed url="https://community.fortinet.com/t5/FortiSwitch/Troubleshooting-Tip-How-to-verify-working-of-storm-control/ta-p/225109" %}





