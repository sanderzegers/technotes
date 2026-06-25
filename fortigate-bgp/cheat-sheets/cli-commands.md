# CLI Commands

| Command                                 | Explanation                                                                                                                                                                                          |
| --------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `execute router restart`                | <p>Restart entire routing engine (static routing, bgp, ospf, etc)<br>Only during maintenance window</p>                                                                                              |
| `execute router clear bgp ip <ip>`      | Restart BGP session to peer with \<ip>. Flushes all BGP routes.                                                                                                                                      |
| `execute router clear bgp ip <ip> soft` | Refreshes routes without tearing down the BGP TCP session. If route refresh is negotiated, FortiGate can request the peer to resend routes; this is useful after changing inbound or outbound policy |
