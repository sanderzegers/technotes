# Best Practices / Security

* Setup default FortiSwitch password

```
config switch-controller switch-profile
 edit default
   set login-passwd-override enable
   set login-passwd <password>
 next
end
```

* Setup MGMT and Internal access: By default https, ping and ssh are allowed. Add SNMP.

```
config switch-controller security-policy local-access
    edit "default"
        set mgmt-allowaccess https ping ssh snmp
        set internal-allowaccess https ping ssh snmp
    next
end
```

* Optionally increase logging level to information or debug:

```
config switch-controller switch-log
    set status enable
    set severity information
end
```

* Disable LLDP ISL Profiles on non FortiLink ports to prevent VLAN-hopping attack

Either manually per port or with:

```
diagnose switch-controller switch-recommendation lock-down-topo-lldp-profile
```



* SNMP Configuration / Monitoring

Monitor core switches and tier switches. ICLs, connections between tiers and FortiLink.

```
config switch-controller snmp-community
    edit 1
        set name "SNMPMonitor"
        config hosts
            edit 1
                set ip 10.1.2.3 255.255.255.255
            next
        end
        set trap-v1-status disable
        set trap-v2c-status disable
        set events cpu-high mem-low log-full intf-ip ent-conf-change
    next
end
```

```
config switch-controller snmp-sysinfo
    set status enable
end
```

```
config switch-controller security-policy local-access
    edit "default"
        set internal-allowaccess https ping ssh snmp radius-acct
    next
end
```

* Enable storm control policy:\
  Default rate is 500, adjust rate to your BUM-Traffic rate.

```
config switch-controller storm-control
    set rate 2000
    set unknown-unicast enable
    set unknown-multicast enable
    set broadcast enable
end
```

{% embed url="https://community.fortinet.com/t5/FortiSwitch/Troubleshooting-Tip-How-to-verify-working-of-storm-control/ta-p/225109" %}

* Lock down the FortiSwitch ICL links to make the automatically created ICLs and ISLs static. \
  In certain situations, the peer switch might not be detected anymore, and without these settings, the system could remove the ICL/ISL, potentially causing a network loop.

<pre><code><strong>diagnose switch-controller switch-recommendation fabric-lockdown-enable
</strong></code></pre>
