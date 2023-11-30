# Loop Protection

{% hint style="info" %}
Loop Guard and Loop Protection are two different methods to prevent loops. See Security section for a description of Loop Guard. Loop Protection builds on top of the Spanning Tree protocol. Loop Guard is recommended in conjugtion with spanning-tree, but is not a prerequisite.
{% endhint %}

Loop protection is build on top of spanning tree, it will prevent loops in some niche cases , eventhough STP is enabled on both sides.

Loop protection ensures that if a blocked port stops receiving the BPDUs that originally caused it to be blocked, it doesn't just start forwarding traffic. This prevents potential network loops in scenarios where, for example, the link to the switch sending those BPDUs fails.

Common cases loop protection can work:

* Unidirectionial link failure (Fiber connection)
  * In a unidirectional link failure, traffic can transmit in one direction on a link, but traffic sent in the opposite direction is lost. This can lead to a scenario where Bridge Protocol Data Units (BPDUs) are not received by one of the switches on a link, causing it to believe the link has failed and start forwarding on what was a blocked port, potentially creating a loop.
* BPDUS expire on blocked port (Software Issue)
* Blocked port eventually starts forwarding traffic
* Loop is created in opposite direction to link failure

Changes to STP with loop protection is enabled on a port:

Blocked port stays in the blocking state if BPDUs are no longer received. It does not transist to forwarding state.

CLI Only setting: (default is disabled)

```
config switch interface
   edit <port>
      set stp-loop-protection enabled
   next
end
```

It's recommended to enable it on all root, alternate and backup ports.
