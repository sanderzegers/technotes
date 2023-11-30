# MAC limit

## Entry types in the MAC address table

**Dynamic**

Learned from the source mac address of incoming packets.

By default the limit is the maximum hardware possible limit (16k,32k,64k entries, see spec sheets).

Dynamic entries are removed from the MAC Table, if the port goes down, the switch reboots, or the aging timer expires if no packets are received anymore (default = 300sec)

**Static**

Configured by the administrator. Added to MAC table and never removed.

**Sticky**

Dynamic entries are converted to static entries. Removed only during switch reboot. Sticky-MACs are also displayed as a 'static' entry in the mac table.



Configure a static MAC:

```
config switch-controller managed-switch
   edit <switch-id>
      config static-mac
         edit 0
            set type static
            set vlan 200
            set mac 00:11:22:33:44:55
            set interface port1
         mext
      end
   next
end
```

Enable sticky mac:

```
config switch-controller managed-switch
   edit <switch-id>
      config ports
         edit <port>
            set sticky-mac enable
         next
      end
   next
end
```

## Limit dynamic entries

This can only be done via CLI, either on a per port basis or per vlan:

Learning limit can be set to 0 (disabled) to maximum 128

{% hint style="danger" %}
Logging for the mac-limit violations is disabled by default. It's recommended to enable.
{% endhint %}

Enable mac limit logging:

```
config switch-controller global
   set log-mac-limit-violations enable
end
```

set mac-limit on a per port basis:

```
config switch-controller managed-switch
   edit S448EFTF23000000
      config ports
         edit port1
            set learning-limit 128
         next
      end
   next
end
```

or per vlan basis:

```
config system interface
   edit VLAN200
      set switch-controller-learning-limit 128
   next
end
```



Verify per CLI:

```
FSW# get switch mac-limit-violations all
```

```
FG# diagnose switch-controller switch-info mac-limit-violations

FG60E_FG1 # diagnose switch-controller switch-info mac-limit-violations all

Managed Switch : S448EFTF23000000 0
      Port              VLAN ID         MAC Address                     Timestamp               Action
---------------------------------------------------------------------------------------------------------
     port1*             200             04:00:00:00:00:78               2023-10-25 17:08:19     none


Managed Switch : S448EFTF23000001 0
      Port              VLAN ID         MAC Address                     Timestamp               Action
---------------------------------------------------------------------------------------------------------
```

Reset mac-limit violation:

```
FG# execute switch-controller switch-action mac-limit-violation reset all S448EFTF23000000
```

Log Entry:

<figure><img src="../.gitbook/assets/grafik (6).png" alt=""><figcaption></figcaption></figure>



