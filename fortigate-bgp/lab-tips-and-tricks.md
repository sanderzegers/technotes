# Lab Tips & Tricks

## SUDO Command

The labs use a lesser-known command, `sudo`. With `sudo`, you can execute commands in another VDOM. For example, if you are currently in VDOM R1 but want to check routes in R2, you would normally execute the following commands:

```
FGT02 # config vdom 

FGT02 (vdom) # edit R1
current vf=R1:1

FGT02 (R1) # get router info routing-table bgp
Routing table for VRF=0
B       10.10.3.0/24 [20/0] via 172.18.13.1 (recursive is directly connected, R1R3-1-0), 00:52:16, [1/0]

FGT02 (R1) # next
FGT02 (vdom) # edit R2
current vf=R2:3

FGT02 (R2) # get router info routing-table bgp
Routing table for VRF=0
B       10.10.3.0/24 [20/0] via 172.18.12.0 (recursive is directly connected, R1R2-1), 00:52:02, [1/0]

FGT02 (R2) # 
```

with sudo you can directly check R2 routes from within the R1 vdom:

```
FGT02 (R1) # get router info routing-table bgp
Routing table for VRF=0
B       10.10.3.0/24 [20/0] via 172.18.13.1 (recursive is directly connected, R1R3-1-0), 00:53:06, [1/0]


FGT02 (R1) # sudo R2 get router info routing-table bgp
Routing table for VRF=0
B       10.10.3.0/24 [20/0] via 172.18.12.0 (recursive is directly connected, R1R2-1), 00:52:52, [1/0]

```

## Packet Capture

It helps a lot to not only rely on log entries and debugging information on the FortiGate, but also to look at what happens under the hood at the network level. Packets never lie.

I highly recommend using my Wireshark Extcap plugin to capture packets live from the FortiGate directly into Wireshark.[https://github.com/sanderzegers/fortigate-extcap](https://github.com/sanderzegers/fortigate-extcap)

