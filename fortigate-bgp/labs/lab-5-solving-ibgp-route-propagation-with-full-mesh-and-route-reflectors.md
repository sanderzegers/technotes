# Lab 5: Solving iBGP Route Propagation with Full Mesh and Route Reflectors

## Objective

In the previous lab we showed that iBGP does not propagate networks out of the box. We show two how approaches on how to achieve this. With a Full Mesh config and with Route Reflectors

## Topology <a href="#topology" id="topology"></a>

<figure><img src="https://github.com/sanderzegers/technotes/raw/refs/heads/undefined/fortigate-bgp/assets/topologies/exports/bgp-lab-master-Lab4.svg" alt=""><figcaption></figcaption></figure>

iBGP Settings:

| Router | AS    | Interface IP                      | BGP Neighbor                      | Loopback Lan Network |
| ------ | ----- | --------------------------------- | --------------------------------- | -------------------- |
| R1     | 65001 | <p>172.18.12.0<br>172.18.13.0</p> | <p>172.18.12.1<br>172.18.13.1</p> | 10.10.1.1/24         |
| R2     | 65001 | 172.18.12.1                       | 172.18.12.0                       | 10.10.2.1/24         |
| R3     | 65001 | 172.18.13.1                       | 172.18.13.0                       | 10.10.3.1/24         |

## Packet Captures <a href="#packet-captures" id="packet-captures"></a>

| PCAP File | Description | What to look for |
| --------- | ----------- | ---------------- |
|           |             |                  |
|           |             |                  |

## Full Mesh iBGP
