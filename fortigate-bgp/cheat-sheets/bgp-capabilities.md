# BGP Capabilities

| Capability               | Meaning                                                                                                                |
| ------------------------ | ---------------------------------------------------------------------------------------------------------------------- |
| Multiprotocol extensions | Allows BGP to carry different address families, such as IPv4 unicast, IPv6 unicast, VPNv4, EVPN, etc.                  |
| Route refresh            | Allows a router to request routes again from a peer without tearing down the BGP session. Useful after policy changes. |
| 4-octet AS number        | Allows BGP to support AS numbers larger than 65535.                                                                    |
| Graceful restart         | Allows forwarding to continue during certain control-plane restarts, if supported and configured.                      |
