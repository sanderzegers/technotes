# Flap Guard

Starting with FortiOS 7.2 this can be configured in the CLI. Before FortiGate 7.2, it can only be configured on the switches.

Can you order the switch to keep the flapguard status of all ports also after rebooting a switch.

This is not supported on all switch models!

```
config switch global
  set flapguard-retain-trigger
end
```

The Flap Guard is a per port setting:

| Setting       | Task                                                       | Default  |
| ------------- | ---------------------------------------------------------- | -------- |
| flapguard     | Enable/disable flap guard.                                 | Disabled |
| flap-rate     | Number of stage change events needed within flap-duration. | 5        |
| flap-duration | Period over which flap events are calculated (seconds).    | 30       |
| flap-timeout  | Flap guard disabling protection (min).                     | 0        |

```
config switch-controller managed-switch
   edit "S448EFTF0000000"
      config ports
        edit "port1"
           set flapguard enable
           set flap-rate 3
           set flap-duration 120
    next
end
```

Show flapguard status:

```
FG60E_FG1 # diagnose switch-controller switch-info flapguard status S448EFTF23000000
Vdom: root Vfid: 0

Managed Switch : S448EFTF23000000       0


  Portname             State      Status       Timeout(m)    flap-rate    flap-duration   flaps/duration  Last-Event
  _________________   _______    _________    ___________    _________    ____________   ______________  ___________

  port1              enabled    Triggered        0          3             120               3     2023-11-08 14:29:09
  port2              enabled      -              0          5             30                0           -
  port3              disabled     -              -          5             30                0           -
```

Log Entry:

<figure><img src="../.gitbook/assets/grafik.png" alt=""><figcaption></figcaption></figure>

To reset the port use following command:

```
FG60# execute switch-controller flapguard reset S448EFTF23000000 port1
Resetting port1 ... OK
```
