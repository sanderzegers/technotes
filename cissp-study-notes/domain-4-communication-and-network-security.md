---
icon: ethernet
---

# Domain 4: Communication & Network Security



| Layer (Number — Name) | Network               |
| --------------------- | --------------------- |
| 7 — Application       | Data                  |
| 6 — Presentation      | Data                  |
| 5 — Session           | Data                  |
| 4 — Transport         | Segments or Datagrams |
| 3 — Network           | Packets               |
| 2 — Data Link         | Frames                |
| 1 — Physical          | Bits                  |



Firewalls active on Network, Session, Application

| OSI Layer(# — Name) | Corresponding TCP/IP Layer | Typical Protocols & Technologies (examples)                   |
| ------------------- | -------------------------- | ------------------------------------------------------------- |
| 7 — Application     | **Application**            | HTTP, HTTPS, SMTP, DNS, FTP, SSH                              |
| 6 — Presentation    | **Application**            | TLS/SSL, X.509, JPEG, MPEG, ASCII/UTF-8                       |
| 5 — Session         | **Application**            | NetBIOS, RPC, SIP, PPTP                                       |
| 4 — Transport       | **Transport**              | TCP, UDP, SCTP                                                |
| 3 — Network         | **Internet**               | IP, ICMP, IGMP, IPv6, IPSec                                   |
| 2 — Data Link       | **Network Access (Link)**  | Ethernet, PPP, 802.11 Wi-Fi, ARP                              |
| 1 — Physical        | **Network Access (Link)**  | UTP/STP cabling, fiber optic, radio (802.11), hubs, repeaters |
|                     |                            |                                                               |

* **Data plane = “packets on the wire.”** Anything that touches every user packet/bit belongs here.
* **Control plane = “brains of the box.”** It decides _where_ those packets should go.
* **Management plane = “human + automation interface.”** It decides _how_ the box itself is set-up, observed, and secured.

North/South traffic: in/out of data center\
East/West traffic: traffic within data center

Air-gapped network: No connectivy to other networks. Access on-site only

Layer 1: Hubs, Repeaters, Concentrators

Concentrator: combine all signals for transmisison down a iingle line



Layer 2: Protocols: ARP, RARP, PPTP, L2TP, L2F



PEAP extension of EAP

SLIP was replaced by PPP

Three authentication protocols were created for PPP: PAP, CHAP and EAP

Happening at Layer 5.

PAP: plaintexc password.\
CHAP: encrypted. challenges are sent in regular intervals.\
EAP: Extensible (!) Authentication Protocol

PEAP: encapsulate EAP in a authenticated and encrypted TLS tunnel



Layer 3: Protocols: ICMP, IGMP, IPsec, OSPF

IPv4: 32-bit (4 bytes) / IPv6 128-bit (16 bytes)



Layer 4: Protocols: TCP, UDP, SSL/TLS

| Well known Ports | Registered Ports (IANA) | Dynanmic / Private |
| ---------------- | ----------------------- | ------------------ |
| 0 - 1023         | 1024 - 49150            | 49151-65535        |
|                  |                         |                    |
|                  |                         |                    |

Layer 5: Protocols: PAP, CHAP, EAP, Netbios, RPC

Layer 5: Circuit proxy firewall,&#x20;

Layer 6: translation, encryption/decryption and compression

* Codecs

Layer 7: Protocols (https, ftp, dns, telnet, ssh



Converged refers to IP network carying non-ip traffic: FCoE, iscsi, voip, SRTP and SIP

iscsi: Internet Small Computer System Interface

pbx: private branch exchange

InfiniBand: RDMA, commonly used by machine learning

Vishing: Voice phishing



Network attack phase: Reconnaissance -> Enumeration -> Vulnerability Analysis -> Exploitation

|                     |                       |                                                                                                                                                                             |
| ------------------- | --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Fragment attacks    | Overlapping fragments | bypass firewall and IDS/IPS by fragmenting, and make the fragments overlapping.                                                                                             |
|                     | Teardrop              | <p>attacker sends fragments of packets of differing sizes, out of order, fake fragment sequnce numbers.<br>Target cannot resseamble packets,  ie using ressource or DoS</p> |
| IP Spoofing Attacks | Smurf                 | ICMP relay attack (source ip spoofing)                                                                                                                                      |
|                     | Fraggle               | port 7 or port 19. CHARGEN (Char code generator) IP spoofing.                                                                                                               |
|                     |                       |                                                                                                                                                                             |

### Wireless

802.11 Wireless protocol family

| **IEEE Standard**      | **Wi-Fi Alliance Name** | **Max Theoretical Speed** | **Frequency Band(s)**       |
| ---------------------- | ----------------------- | ------------------------- | --------------------------- |
| 802.11                 | -                       | 2 Mbps                    | 2.4 GHz                     |
| 802.11a                | Wi-Fi 2 (retroactive)   | 54 Mbps                   | 5 GHz                       |
| 802.11b                | Wi-Fi 1 (retroactive)   | 11 Mbps                   | 2.4 GHz                     |
| 802.11g                | Wi-Fi 3 (retroactive)   | 54 Mbps                   | 2.4 GHz                     |
| 802.11n                | Wi-Fi 4                 | 600 Mbps (theoretical)    | 2.4 GHz and 5 GHz           |
| 802.11ac               | Wi-Fi 5                 | 1.3 Gbps – 6.9 Gbps       | 5 GHz                       |
| 802.11ax               | Wi-Fi 6 / Wi-Fi 6E      | Up to 9.6 Gbps            | 2.4 GHz, 5 GHz, and 6 GHz\* |
| 802.11be (in progress) | Wi-Fi 7 (upcoming)      | Up to 46 Gbps (projected) | 2.4 GHz, 5 GHz, and 6 GHz   |
|                        |                         |                           |                             |
|                        |                         |                           |                             |

ad hoc mode: WEP only. Client to Client without AP

succeeded by Wifi-Direct (WPA2 and WPA3)

EAP:

* One factor: EAP-MD5, LEAP, PEAP-MSCHAP, TTLS-MSCHAP, EAP-SIM
* Two-factor: EAP-TLS, TTLS with OTP, PEAP-GTC



Wireless encryption:

* TKIP (Temporal Key Integrity Protocol). Replacement for WEP.
* CCMP (Counter-Mode-CBC-MAC Protocol): AES with 128-bit key WPA2 and WPA3



Wireless integrity protocol:

* TKIP (message integrity code called "Michael."
* WPA2 uses CCMP (AES in CBC-MAC mode)



Application plane\
\- Northbound APIs -\
Control Plan\
\- Southbound APIs -\
Data Plan

Defense in depth: (layers outside to inside)

* policies and procedures
* environmental considerations
* physical infrastructure
* operating system
* software configurations



Partitioning

Choke point - DMZ - Boundary Router (simple DMZ concept)



packet filtering firewall & stateful packet filtering layer 3 and 4 (transport & network layer)\
circuit proxy firewall layer 5 (session)\
application proxy firewall layer 7 (application)

circuit proxy firewall: socks5

CBAC = context-based access control.\
Firewall makes decission based on application layer protocol session information



Definition of Enticement and Entrapment in the context of honeypots:

Enticement: Legal activity of persuading someone to commit a crime that they were already planning to commit

Entrapment: Illegal activity of persuading someone to commit a crime that they would not otherwise have commited.

\
GRE + L2TP no encryption, just tunneling

AH: Authentication, Integrity, data-origin auth, replay protection\
ESP: Everything AH does + encryption (but you need a mode that includes authentication (AES-GCM)



Transport mode: keep original IP headers\
Tunnel mode: replace headers and encapsulate



IKE: Internet key exchange. a version of Diffie-Hellman and is used by IPSec to generate the same session key&#x20;

Security Association: one-way establishment of attributes at thes tart of communication between two entities.\
If IPSEC and AH are required. A total of 4 SAs are established.\
Attributes are:

* authentication algo
* crypto algo
* encryption keys
* mode (transport/tunnel)
* sequence number
* Expirty of SA

