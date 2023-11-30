# Fortigate Built-In switch

The Fortigate Built-in switches are not comparable with FortiSwitch models. The supported features are very limited.

The recommended method for connecting a FortiSwitch to a FortiGate involves configuring a Trunk port, which is also the current factory default setting. Earlier versions of FortiGate did not support LACP Trunks on lower-end models, requiring the use of the built-in switch.

Fortigate supports three type of built-in switches:

* Software Switch
* Hardware Switch
* VLAN Switch

The Software switch is mainly meant to be used when combining virtual and physical interfaces. A typical usecase is to bridge tunneled SSIDs to an existing VLAN / Network.\
Software switches cannot offload any traffic to the hardware. So all sessions passing the Software Switch are running on the CPU.

The Hardware Switch is bound to the hardware network interfaces on the Fortigate.

Virtual VLAN Switch

Allows to define a VLAN ID for ports in a Hardware Switch. Only untagged ports are possible. Additionially you can define one interface as "trunk" interface, where all VLANs are tagged.

This allows following setup for example. Port15 is defined as "trunk", two different VLANs are configured - one for each ISP.

![](<.gitbook/assets/f7b119fbf3ddcf993dc75e6868d6fdb3\_HA topology-01.png>)

Hardware Switch vs Software Switch

| Feature              | Hardware switch                                                                               | Software switch                                                |
| -------------------- | --------------------------------------------------------------------------------------------- | -------------------------------------------------------------- |
| Processing           | Packets are processed in hardware by the hardware switch controller, or SPU where applicable. | Packets are processed in software by the CPU.                  |
| STP                  | Supported                                                                                     | Not Supported                                                  |
| Wireless SSIDs       | Not Supported                                                                                 | Supported                                                      |
| Intra-switch traffic | Allowed by default.                                                                           | Allowed by default. Can be explicitly set to require a policy. |
| VLANs                |                                                                                               | Not Supported                                                  |



Todo: Traffic between different Hardware Switches is allowed?
