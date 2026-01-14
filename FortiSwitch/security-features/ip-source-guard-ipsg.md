# IP Source Guard (IPSG)

IPv4 Source Guard is a security feature that protects networks from IPv4 address spoofing by ensuring that only traffic from specified IPv4 addresses is permitted on a given port. Traffic originating from any other IPv4 addresses is promptly discarded. However, it is important to note that addresses which are discarded are not logged or recorded.

IPv4 Source Guard considers two types of address entries:

1. **Static Entries**: These are IP addresses that have been manually mapped to MAC addresses by network administrators. Such entries are generally considered more trusted and reliable.
2. **Dynamic Entries**: These are IP addresses dynamically learned through the DHCP snooping process. DHCP snooping tracks the IP addresses that are automatically assigned to devices on a network, providing an additional layer of security.

DHCP snooping is not required for IPSG, but it is recommended.

IPv4 Source Guard is not enabled by default. For protection to be effective, it must be explicitly enabled on each individual port that requires safeguarding against IP spoofing threats.

There is a limit to the number of IP Source Guard entries that can be made from a FortiGate unit. The maximum is 2,048 entries; attempting to add more will result in an error. In situations where there is an overlap between static and dynamic entries, the static entries are given priority and will override the dynamic ones.

Configuration of IPv4 Source Guard is exclusively available within FortiOS for the management of FortiSwitch units that are equipped with IP Source Guard capability. Check the feature matrix if the feature is available.

Configure static entry:

<pre><code>config switch-controller managed-switch
    edit &#x3C;FortiSwitch_serial_number>
<strong>        config ip-source-guard
</strong>            edit &#x3C;port_name>
                config binding-entry
                edit &#x3C;id>
                    set ip &#x3C;xxx.xxx.xxx.xxx>
                    set mac &#x3C;XX:XX:XX:XX:XX:XX>
                    next
                end
            next
        end
    next
end
</code></pre>

To enable IPv4 Source guard:

```
config switch-controller managed-switch
   edit "S448EFTF0000000"
      config ports
        edit "port1"
           set ip-source-guard enable
    next
end
```

View IPSG:

```
FG60E_FG1 # diagnose switch-controller switch-info ip-source-guard hardware S448EFTF23000000

Managed Switch : S448EFTF23007654 0
Filter:
  IP: none
  Interface: none
  MAC: none
   IPv4 Address |                Interface(ID) |               MAC | Type
----------------+------------------------------+-------------------+------------
  192.168.100.3 |            port1(0x08000001) | 00:0c:29:01:22:33 | dynamic

```

Or directly on the FortiSwitch:

```
FSW# get switch ip source guard violations all
```

{% hint style="warning" %}
Logging is not enabled by default for IPSG. Recommended to enable
{% endhint %}

It must be configured on the FortiSwitch. Unfortunately it's not possible to configure via the Fortigate.

```
config switch global
  set log-source-guard-violations enable
  set source-guard-violation-timer <minutes>
```

It's also possible to let source-guard reset it self automatically. This will generate a log entry everytime the source-guard times out. By default the time is set to 0, which means that IPSG violations won't expire.

View IPSG violation events:

<figure><img src="../.gitbook/assets/grafik (2).png" alt=""><figcaption></figcaption></figure>

