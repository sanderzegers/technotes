# Quarantine

Two modes are available:

VLAN mode (default):

MAC address is moved into the Quarantine VLAN. The default quarantine VLAN has a dhcp server configured and no firewall policies. Devices in the Quarantine network, can communicate to each other, but by default to nowhere else.\
Technically the Fortigate configures a mac to vlan mapping on the Fortiswitch.



Redirect Mode:

Devices stays in VLAN, but is added to the QuarantinedDevices firewall address group. Block policies must be configured on the firewall to make this useful.

```
config switch-controller global
   set quarantine-mode by-vlan | by-redirect
```

Add devices to the Quarantine by right-click on the device and select "Quarantine Host"

<figure><img src="../.gitbook/assets/grafik (7).png" alt=""><figcaption></figcaption></figure>

And can be removed in the same way as well.

An overview of all quarantined devices is available as a dashboard: Dashboard -> User & Devices -> Quarantine

FortiSwitch Log entry:

<figure><img src="../.gitbook/assets/grafik (9).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
This feature can be very powerful, by combing FortiAnalyzer automation task. You can create a quarantine action based on Log Events. Eg. port scan detected.
{% endhint %}
