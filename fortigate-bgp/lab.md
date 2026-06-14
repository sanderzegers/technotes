---
description: Lab Revision 1 / 6.2026
---

# Lab

## FortiGate BGP Lab — Baseline Configuration Documentation

### 1. Lab Scope

This document describes the baseline FortiGate VDOM lab configuration. The lab is targeted at demonstrating, testing, and validating OSPF and BGP routing deployments in a controlled environment.

The topology includes multiple connectivity types so that different routing scenarios can be tested from the same baseline setup:

| Connectivity Type             | Purpose                                                                                         |
| ----------------------------- | ----------------------------------------------------------------------------------------------- |
| Point-to-point links          | Used for routed links between individual router VDOMs                                           |
| Parallel point-to-point links | Used to test redundancy, equal-cost paths, failover, and path selection                         |
| Shared Ethernet segment       | Used to test broadcast-network behavior, multi-access routing, and shared BGP peering scenarios |
| Loopback interfaces           | Used for router IDs, stable routing endpoints, and loopback-based protocol testing              |
| Internal mesh connectivity    | Used to test intra-domain routing and multiple-path scenarios                                   |
| Stub connectivity             | Used to simulate customer, branch, or downstream networks                                       |
| Transit connectivity          | Used to simulate upstream, provider, or multi-AS routing paths                                  |

The lab can be run on a single FortiGate appliance, either physical hardware or a virtual FortiGate appliance. It consists of a ten-VDOM deployment: nine router VDOMs and one infrastructure VDOM.

The router VDOMs are named `R1` through `R9`. The `root` VDOM is used as the infrastructure VDOM and hosts shared components such as the virtual software switch for the shared Ethernet segment.



### 2. Topology

<figure><img src=".gitbook/assets/Architecture-1.drawio.png" alt=""><figcaption></figcaption></figure>

### 3. Addressing Plan

| Purpose              |          Prefix |    Prefix Length | Notes                        |
| -------------------- | --------------: | ---------------: | ---------------------------- |
| Loopback interfaces  | `172.17.0.0/16` | `/32 per router` | One loopback per router VDOM |
| Point-to-point links | `172.18.0.0/16` |   `/31 per link` | Used for PPP VDOM links      |
| Shared Ethernet LAN  | `172.19.0.0/24` |            `/24` | Used for `NET1`              |
| Simulated LANs       |   `10.1.0.0/16` | `/24 per router` | Simulate local conneted Nets |

### 4. Loopback Interfaces

| VDOM | Interface Name |   IP Address |           Netmask |
| ---- | -------------- | -----------: | ----------------: |
| `R1` | `R1-LO0`       | `172.17.0.1` | `255.255.255.255` |
| `R2` | `R2-LO0`       | `172.17.0.2` | `255.255.255.255` |
| `R3` | `R3-LO0`       | `172.17.0.3` | `255.255.255.255` |
| `R4` | `R4-LO0`       | `172.17.0.4` | `255.255.255.255` |
| `R5` | `R5-LO0`       | `172.17.0.5` | `255.255.255.255` |
| `R6` | `R6-LO0`       | `172.17.0.6` | `255.255.255.255` |
| `R7` | `R7-LO0`       | `172.17.0.7` | `255.255.255.255` |
| `R8` | `R8-LO0`       | `172.17.0.8` | `255.255.255.255` |
| `R9` | `R9-LO0`       | `172.17.0.9` | `255.255.255.255` |

### 5. Local / Simulated Site Networks

These interfaces provide local routed prefixes for each router VDOM. They are implemented as loopback interfaces and can be used as simple networks to advertise during OSPF or BGP labs.

| VDOM | Interface Name | Local Network | Interface IP |         Netmask |
| ---- | -------------- | ------------: | -----------: | --------------: |
| `R1` | `R1-LAN0`      | `10.1.1.0/24` |   `10.1.1.1` | `255.255.255.0` |
| `R2` | `R2-LAN0`      | `10.1.2.0/24` |   `10.1.2.1` | `255.255.255.0` |
| `R3` | `R3-LAN0`      | `10.1.3.0/24` |   `10.1.3.1` | `255.255.255.0` |
| `R4` | `R4-LAN0`      | `10.1.4.0/24` |   `10.1.4.1` | `255.255.255.0` |
| `R5` | `R5-LAN0`      | `10.1.5.0/24` |   `10.1.5.1` | `255.255.255.0` |
| `R6` | `R6-LAN0`      | `10.1.6.0/24` |   `10.1.6.1` | `255.255.255.0` |
| `R7` | `R7-LAN0`      | `10.1.7.0/24` |   `10.1.7.1` | `255.255.255.0` |
| `R8` | `R8-LAN0`      | `10.1.8.0/24` |   `10.1.8.1` | `255.255.255.0` |
| `R9` | `R9-LAN0`      | `10.1.9.0/24` |   `10.1.9.1` | `255.255.255.0` |

### 6. Point-to-Point VDOM Links

| Link           | VDOM Link Name | Type  |           Subnet | Side 0 Interface |     Side 0 IP | Side 1 Interface |     Side 1 IP |
| -------------- | -------------- | ----- | ---------------: | ---------------- | ------------: | ---------------- | ------------: |
| R1 ↔ R2        | `R1R2-`        | `ppp` | `172.18.12.0/31` | `R1R2-0`         | `172.18.12.0` | `R1R2-1`         | `172.18.12.1` |
| R1 ↔ R3 link 1 | `R1R3-1-`      | `ppp` | `172.18.13.0/31` | `R1R3-1-0`       | `172.18.13.0` | `R1R3-1-1`       | `172.18.13.1` |
| R1 ↔ R3 link 2 | `R1R3-2-`      | `ppp` | `172.18.13.2/31` | `R1R3-2-0`       | `172.18.13.2` | `R1R3-2-1`       | `172.18.13.3` |
| R1 ↔ R4        | `R1R4-`        | `ppp` | `172.18.14.0/31` | `R1R4-0`         | `172.18.14.0` | `R1R4-1`         | `172.18.14.1` |
| R2 ↔ R3        | `R2R3-`        | `ppp` | `172.18.23.0/31` | `R2R3-0`         | `172.18.23.0` | `R2R3-1`         | `172.18.23.1` |
| R2 ↔ R4        | `R2R4-`        | `ppp` | `172.18.24.0/31` | `R2R4-0`         | `172.18.24.0` | `R2R4-1`         | `172.18.24.1` |
| R3 ↔ R4        | `R3R4-`        | `ppp` | `172.18.34.0/31` | `R3R4-0`         | `172.18.34.0` | `R3R4-1`         | `172.18.34.1` |
| R3 ↔ R9        | `R3R9-`        | `ppp` | `172.18.39.0/31` | `R3R9-0`         | `172.18.39.0` | `R3R9-1`         | `172.18.39.1` |
| R5 ↔ R6        | `R5R6-`        | `ppp` | `172.18.56.0/31` | `R5R6-0`         | `172.18.56.0` | `R5R6-1`         | `172.18.56.1` |
| R6 ↔ R7        | `R6R7-`        | `ppp` | `172.18.67.0/31` | `R6R7-0`         | `172.18.67.0` | `R6R7-1`         | `172.18.67.1` |

### 7. NET1 Shared Ethernet Segment

`NET1` is a virtual software switch in the `root` VDOM. It simulates a broadcast domain where multiple routers can reach each other.

| Parameter              |            Value |
| ---------------------- | ---------------: |
| Switch name            |           `NET1` |
| VDOM                   |           `root` |
| Subnet                 |  `172.19.0.0/24` |
| Connected router VDOMs | `R4`, `R5`, `R8` |
