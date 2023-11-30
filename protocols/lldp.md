# LLDP

LLDP is enabled out of the box on FortiSwitch.

Ethernet Type 0x88CC

\
\- by default send every 30 secs, if switch is in managed mode than every three seconds by default because of the auto-isl feature.

LLDP is used for following purposes:

* Layer 2 discovery: FSW maintains a list of LLDP neighbors an their capabillities
* Alternative FortiSwitch discovery method
* Auto-ISL: Form trunks between adjecent FortiSwitches automatically. This happens as soon two switches are connected. No authorization on the Fortigate is required for this step.

FortiSwitch forwards LLDP updates to Fortigate using CAPWAP to feed the Fortigate device detection daemon



TLVs

Type 1



Type 2

Type 3
