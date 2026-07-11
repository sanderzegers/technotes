# iBGP vs eBGP Quick Comparison

<table data-search="false"><thead><tr><th>Topic</th><th>iBGP</th><th>eBGP</th></tr></thead><tbody><tr><td>AS relationship</td><td>Peers are in the same AS</td><td>Peers are in different ASes</td></tr><tr><td>Typical use</td><td>Inside one administrative domain</td><td>Between separate administrative domains</td></tr><tr><td>Remote AS setting</td><td><code>remote-as</code> is the same as the local AS</td><td><code>remote-as</code> is different from the local AS</td></tr><tr><td>Route advertisement rule</td><td>Routes learned from one iBGP peer are not advertised to another iBGP peer by default</td><td>Routes learned from one eBGP peer can be advertised to another eBGP or iBGP peer, subject to policy</td></tr><tr><td>Main loop prevention method</td><td>Split-horizon style rule for iBGP-learned routes</td><td>AS_PATH loop detection</td></tr><tr><td>AS_PATH behavior</td><td>Local AS is not added when advertising to another iBGP peer</td><td>Local AS is added when advertising to an eBGP peer</td></tr><tr><td>LOCAL_PREF</td><td>Commonly used and propagated inside the AS</td><td>Not advertised to external peers</td></tr><tr><td>NEXT_HOP behavior</td><td>Often preserved, so next-hop reachability must exist in the underlay</td><td>Usually changed to the advertising router on the eBGP hop</td></tr><tr><td>Scalability</td><td>Full mesh does not scale well; route reflectors are common</td><td>Does not require full mesh between all peers</td></tr><tr><td>Administrative distance on FortiGate</td><td>200 by default</td><td>20 by default</td></tr><tr><td>Common design need</td><td>Underlay reachability, route reflectors, <code>next-hop-self</code> style behavior</td><td>Policy control between ASes, filtering, prepending, communities, MED</td></tr></tbody></table>

### Main operational differences

* iBGP is used to carry BGP information inside one AS.
* eBGP is used to exchange routes between different ASes.
* iBGP does not re-advertise iBGP-learned routes to other iBGP peers by default.
* eBGP advertises routes across AS boundaries and extends the AS\_PATH.
* iBGP often depends on separate underlay routing so the BGP next hop stays reachable.
* eBGP usually uses the directly connected peer as the next hop on that hop.

### What to remember in this lab guide

* Labs 1 and 2 compare basic session establishment.
* Lab 3 compares route origination and basic path attributes.
* Lab 4 shows that eBGP re-advertises routes while iBGP does not.
* Lab 5 shows how full mesh and route reflectors solve iBGP propagation, and why next-hop reachability still matters.
