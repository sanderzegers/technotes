# Quarantine

Two Fortigate Quarantine modes are available:

**VLAN mode (default):**

MAC address is moved into the Quarantine VLAN. The default quarantine VLAN has a DHCP server configured and no firewall policies. Devices in the quarantine network, can communicate to each other, but by default to nowhere else.\
Technically the Fortigate configures a mac to vlan mapping on the Fortiswitch.

**Redirect Mode:**

Devices stay in their configured VLAN, but are added to the **QuarantinedDevices** firewall address group. Block policies must be configured on the firewall to make this useful.

```
config switch-controller global
   set quarantine-mode by-vlan | by-redirect
```

Quaranting devices or remove them from the Quarantine, by right-click on the device and select "Quarantine Host":

<figure><img src="../.gitbook/assets/grafik (7).png" alt=""><figcaption></figcaption></figure>

An overview of all quarantined devices is available as a dashboard: Dashboard -> User & Devices -> Quarantine

FortiSwitch Log entry:

<figure><img src="../.gitbook/assets/grafik (9).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
This feature becomes highly effective when combined with FortiAnalyzer automation tasks, allowing for the creation of quarantine actions based on specific Log Events, such as when a port scan is detected.
{% endhint %}
