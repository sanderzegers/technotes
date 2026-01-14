# Protocols

#### FortiLink:

Ethernet type 0x88ff

Default discovery method. Runs on Fortilink (between Fortigate and FortiSwitch)\
Heartbeats\
Discovery Frames<br>

FortiSwitch Servicename: fortilinkd\
Fortigate Servicename:

#### LLDP:

LLDP Protocol (Ethernet Type 0x88cc)

Can be used as alternative for FortiSwitch discovery\
Used for auto-isl configuration\
Runs by default on all ports

FortiSwitch Servicename: lldpmedd<br>

#### MCLAG:

Protocol runs on ICL between Chassis members.\
Synchronization of MCLAG daemon

FortiSwitch Servicename: l2d / l2dbg



#### CAPWAP:

DTLS Tunnel

Switch authentication and Authorization\
Heartbeats\
Alternative Firmware upgrade method\
LLDP neighbor info

FortiSwitch Servicename: cu\_swtpd\
Fortigate Servicename: cu\_acd<br>

#### **API:**

HTTPS

Push switch configuration from Fortigate\
Various status options?

#### NTP:

Time synchronization. Fortiswitches do not include an internal timer, after each reboot time must be set.\
Time synchronization necessary for CAPWAP (DTLS) Traffic!



#### STP:

Block network loops, by disabling redudant network paths

#### DHCP:

IP address assignment of FortiSwitches



#### DNS:

