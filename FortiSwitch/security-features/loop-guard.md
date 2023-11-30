# Loop Guard

Loop Guard is a loop prevent feature.

* Can detect more loop cases than loop protection
* Does not rely on spanning tree BPDUs and port states
* Designed to work in conjuction of STP, not as a replacement
* Disabled by default
* Fortinet proprietary protocol
* loop guard periodically broadcasts loop guard frame on the Native VLAN of a port
* Will shutdown port, if frame is received on switch
* Orginal loop guard implementation didn't acount for loops other than the native VLAN
* For this reason loop guard feature was improved to include the MAC Move option



* Mac move monitors repeated MAC address flapping events, which are likely caused by a loop
* To enanble mac move, define threshold
* threshold refers to minimum number of MAC addresses that must flap between ports within a second
* Be careful before activating mac move and setting threshold if you have
  * NAC enabled
  * Wireless Bridge Mode

Loop Guard packet (LPBDU):

<figure><img src="../.gitbook/assets/grafik (19).png" alt=""><figcaption><p>f</p></figcaption></figure>

Log entry witch-controller : `Loop Guard: loop detected on port2. Shutting down port2.`

CLI on Switch:

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

Process is called /bin/lpgd



