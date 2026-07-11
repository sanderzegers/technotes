# BGP Best-Path Algorithm

1. Prefer the path with the highest WEIGHT.
2. Prefer the path with the highest [LOCAL\_PREF](https://www.cisco.com/en/US/tech/tk365/technologies_tech_note09186a00800c95bb.shtml#localpref).
3.  Prefer the path that was locally originated via a **network** or **aggregate** BGP subcommand or through redistribution from an IGP.

    Local paths that are sourced by the [**network**](https://www.cisco.com/en/US/tech/tk365/technologies_tech_note09186a00800c95bb.shtml#networkcommand) or **redistribute** commands are preferred over local aggregates that are sourced by the [**aggregate-address**](https://www.cisco.com/en/US/tech/tk365/technologies_tech_note09186a0080094826.shtml) command.
4. Prefer the path with the shortest AS\_PATH.
5. Prefer the path with the lowest origin type.
6. Prefer the path with the lowest [multi-exit discriminator (MED)](https://www.cisco.com/en/US/tech/tk365/technologies_tech_note09186a0080094934.shtml#med).
7. Prefer eBGP over iBGP paths.
8. Prefer the path with the lowest IGP metric to the BGP next hop.
9. Prefer the oldest path when both paths are eBGP
10. ~~Lowest router ID~~
11. Shortest Cluster list length
12. Lowest neighbor address



By default Fortigate disables lowest router id:

```
BGP-LAB (bgp) # get | grep bestpath
bestpath-as-path-ignore: disable 
bestpath-cmp-confed-aspath: disable 
bestpath-cmp-routerid: disable 
bestpath-med-confed : disable 
bestpath-med-missing-as-worst: disable 

```
