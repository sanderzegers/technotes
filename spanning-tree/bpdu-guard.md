# BPDU Guard

BPDU Guard, will block a port as soon as it detects Spanning Tree packets (BPDU).\
This means in practice, when a managed switch with Spanning-Tree protocol is connected, and it's not a FortiSwitch, it will shutdown the port.\
Clients should not send spanning-tree packets.

It's a per port setting and can be enabled in the GUI:

<figure><img src="../.gitbook/assets/grafik (17).png" alt=""><figcaption></figcaption></figure>

In the CLI it's possible to change the bpdu-guard timeout from the default 5min value to another:

```
config switch-controller managed-switch
    edit <switch-id>
        config ports
            edit <port>
                set stp-bpdu-guard enabled
                set stp-bpdu-guard-timeout <mins>
            next
        end
    next
end 
```

Display BPDU Guard status for each switch

```
G60E # diagnose switch-controller switch-info bpdu-guard-status  S448EFTF23007146
Vdom: root
Managed Switch : S448EFTF23001234 0


  Portname             State      Status       Timeout(m)    Count    Last-Event
  _________________   _______    _________    ___________    _____   __________________

  port2              disabled       -              -             -            -
  port3              disabled       -              -             -            -
  port4              disabled       -              -             -            -
  port5              disabled       -              -             -            -
  port6              disabled       -              -             -            -
  port7              disabled       -              -             -            -
  port8              disabled       -              -             -            -
  port9              disabled       -              -             -            -
  port10             disabled       -              -             -            -
  port11             disabled       -              -             -            -
  port12             disabled       -              -             -            -
  port13             enabled      Triggered        5             2     2023-10-15 14:41:04
  port14             enabled        -              5             0            -
  port15             enabled        -              5             0            -
```

Reset BPDU Guard:&#x20;

```
FG60E # execute switch-controller switch-action bpdu-guard reset <switch-id> <port>
```

Log Entries:

<figure><img src="../.gitbook/assets/grafik (18).png" alt=""><figcaption></figcaption></figure>
