# FortiLink Architecture

## Managed FortiSwitch

Three methods are available to manage and configure FortiSwitches.&#x20;

\- Standalone mode\
\- Cloud Managed\
\- Fortigate Managed FortiSwitch.&#x20;

The factory default settings is standalone mode.

In standalone mode, the switch is managed by GUI or CLI. Configuration are made on each FortiSwitch independently.

When deployed as a managed switch, the Fortigate acts as an controller. Managed switch mode is also known as Fortilink mode.

A comprehensive list detailing the features supported by the various switch models is available here. The availability of these features varies depending on the hardware model and whether the switch is operating in standalone or managed mode.

{% embed url="https://docs.fortinet.com/document/fortiswitch/7.4.1/fortiswitchos-feature-matrix" %}

The fortiLink interface on the Fortigate is used to manage the FortiSwitch stack, and to process inter-VLAN traffic.

Multiple FortiLinks can be created. Fortigates in standalone, HA Active Passive and HA Active-Active are supported.&#x20;

## Traffic flow

A FortiLink setup handles traffic like a classic router-on-a-stick design.\
Intra-VLAN traffic is handled by the switch stack: traffic destinated in the same VLAN is not passing through the fortigate by default (see Block-intra VLAN Traffic).\
Any other traffic must pass the Fortigate.\
FortiLink management traffic is sent untagged on VLAN 4094 by default. VLAN 4094 is untagged on all ISLs, ICL and FortiLink Trunks.\


Auto ISL creation

* Parameters
* Default timeout values

**FortiLink**: Static trunk\
**ICL and ISL** are LACP-active Trunks.



Fortigate requests two addresses via DHCP: One for management and depending on the Switch Model one for (E)RSPAN

Possible Fortigate to FortiSwitch connectivity methods:

* Single connection
* Hardware Switch / Software Switch
* LACP connection

Switch Management Traffic VLAN 4094 by default is configured as native VLAN on the FortiLink Trunk, ISL and ICL.

The default IP range for the FortiLink port is 10.255.0.1/16. The Fortigate becomes DNS and NTP server for the FortiLink.

Switches must be authorized on the Fortigate before they become manageable. Alternativelly it's possible to configure "automically authorize devices" globaly. FortiLink split interface is enable by default. Split interface, will only keep one FortiLink interface member active. This is useful if MCLAG is not configured yet, or cannot be configured.



## FortiLink Auto-Discovery

Fortigate can discover FortiSwitch regardless of switch management mode.

In FortiSwitchOS 3.4.0 and later releases, the last four ports are the default auto-discovery FortiLink ports. You can enable additional ports by setting the auto-discovery-fortilink parameter:

```
config switch interface
edit <port>
    set auto-discovery-fortilink enable
end
```

FortiSwitch 7.2.0 and later have FortiLink auto-discovery enabled on all ports.

After FortiSwitch gets authorized by the Fortigate, the FortiSwitch switches to managed Switch mode. It will lose all configurations made in standalone mode. This information will be restored, if you decide to change back to standalone mode again.

FortiSwitches can be authorized through the Fortigate GUI:

Or via command

```
config switch-controller managed-switch
   edit <switchid>
      set fws-wan1-admin enable
   next
end
```

The Fortigate will send a FortiLink Discover response and the switch will change from local mode to FortiLink managed mode.



## Default FortiSwitch VLANs



| VLAN Name    | VLAN ID | Description                                                                                                          |
| ------------ | ------- | -------------------------------------------------------------------------------------------------------------------- |
| Default      | 1       | Native VLAN. Default VLAN configured on the FortiSwitches                                                            |
| Quarantine   |         | Default VLAN used for quarantined traffic. (Quarantine device action on the Fortigate)                               |
| Rspan        |         | Packet Capture VLAN (RSPAN and ERSPAN)                                                                               |
| Voice        |         | When using LLDP-Media Endpoint Discovery (LLDP-MED), you can assign the switch port to this VLAN.                    |
| Video        |         | When LLDP detected video device                                                                                      |
| Onboarding   |         | When NAC is enabled, this is the VLAN where devices that do not match any of the configured NAC policies are placed. |
| Nac\_segment |         | This is used for NAC VLAN segmentation.                                                                              |



A DHCP Server is configured for All VLANs except the 'default' VLAN.

It's possible to delete or add VLANs to the default settings.

