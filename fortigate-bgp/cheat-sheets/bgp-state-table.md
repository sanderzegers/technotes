# BGP State table

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

Source: [https://www.logicmonitor.com/deep-dive/bgp-monitoring/bgp-states](https://www.logicmonitor.com/deep-dive/bgp-monitoring/bgp-states)

| States      | Description                                                        |
| ----------- | ------------------------------------------------------------------ |
| Idle        | BGP is not currently trying, or it is waiting before retrying.     |
| Connect     | BGP is trying to establish the TCP session.                        |
| Active      | BGP is still trying to connect. Often means TCP session is failing |
| OpenSent    | TCP is up and BGP as sent an OPEN message                          |
| OpenConfirm | BGP parameters were accepted and the session is almost established |
| Established | BGP sesion is up and routes can be exchanged                       |

|   |
| - |

| BGP is still trying to connect. This often means the TCP session is failing. |
| ---------------------------------------------------------------------------- |

|   |
| - |

| BGP is not currently trying, or it is waiting before retrying. |
| -------------------------------------------------------------- |
