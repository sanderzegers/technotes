# Loop Guard

Loop Guard is a feature designed to prevent network loops. \


It is capable of detecting more loop scenarios than standard loop protection, as it does not rely on Spanning Tree Protocol BPDUs or port states. \
While it is designed to complement STP, it is not intended as a replacement. \


By default, Loop Guard is disabled and is a proprietary protocol of Fortinet.\
Loop Guard functions by periodically broadcasting a Loop Guard frame on the Native VLAN of a port. If this frame is received back on the switch, the port will be shut down.&#x20;

The original implementation of Loop Guard did not account for loops in VLANs other than the native one. Therefore, the Loop Guard feature was enhanced to include the MAC Move option to address this limitation.

MAC move monitors repeated MAC address flapping events, often indicative of a loop.&#x20;

To enable MAC move, a threshold must be defined. This threshold is the minimum number of MAC addresses required to flap between ports within one second.&#x20;

Exercise caution when activating MAC move and setting the threshold, especially if NAC or Wireless Bridge Mode is enabled.

Following Screenshot shows a Loop Guard packet (LPBDU):

<figure><img src="../.gitbook/assets/grafik (19).png" alt=""><figcaption><p>f</p></figcaption></figure>

When a network loop is detected by Loop Guard following message as added to the FortiSwitch logs: \
`Loop Guard: loop detected on port2. Shutting down port2.`

Show loop-guard on FortiSwitch:

```
SWITCH03 # diagnose loop-guard status


  Portname             State     Status     Timeout(m)   MAC-Move   Count    Last-Event
  _________________   _______   _________   __________   ________   _____   __________________

  port1              disabled    -             -           -         -            -
  port2              enabled   Triggered       45          0         1     2023-09-22 15:58:17
  port3              disabled    -             -           -         -            -
  port4              disabled    -             -           -         -            -
  port5              disabled    -             -           -         -            -
```

Show loop-guard status via Fortigate:

```
diagnose switch-controller switch-info loop-guard
```

The port cannot be re-enabled through the Fortigate GUI. Either the port must be reset via CLI or wait for timeout to reset the loop-guard status.

```
SWITCH03 # execute loop-guard reset port2
Resetting port2 ... OK
```

```
FG60E # execute switch-controller switch-action loop-guard reset S448EFTF23000000 port2 
Resetting port2 ... OK
```

Default timeout is 45min, this can be changed per Port to 0 - 120min

```
config switch-controller managed-switch
    edit "S448EFTF23000000"
        config ports
            edit "port2"
                set loop-guard enabled
                set loop-guard-timeout 1
            next
```

Change Loop Guard packet interval. Defaulft is every 3 sec

```
config switch global
 set loop-guard-tx-interval 3
```

The Process on the FortiSwitch is named /bin/lpgd



