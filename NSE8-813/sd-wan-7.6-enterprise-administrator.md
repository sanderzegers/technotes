# SD-WAN 7.6 Enterprise Administrator

## Questions:

Difference ADVPN 1.0 and ADVPN 2.0

SDWAN / Path selecting, all settings (service-sla-tie-break)





## To Lab

Use Wizard & compare to jinja rollout

Update Static Route & Cascade Interfaces

Passive / Active SLA Monitoring



Additional Reading:&#x20;

Deployment Guide for MSSP



## Notes

Zerotouch provisioning

Onboarding methods:

ZTP using FortiCloud and FortiDeploy:

* Gate contacts FortiCloud, authenticates using the S/N, connects to FortiManager

ZTP using DHCP option:

* Retrieve FortiManager IP and Credentials through DHCP Option 240/241 on WAN interface

ZTP using USB Boot

* Obtain initial config through usb stick

LTP (Low touch provisioning): Manually configure FM IP





ZTP Workflow

SD-WAN overlay templates:

* Metadata variables
* CLI templates
* IPSec templates
* BGP templates
* Policy packages

Load FG serial numbers and device names into FM (import CSV)

FMG auto-accept devices and pushes configuration



Device blueprints

* enforce firmware version
* add device to group
* pre-run cli template
* assign policy package
* assign provisioning template





## Commands

| Command                          | Description                                             |
| -------------------------------- | ------------------------------------------------------- |
| get ipsec tunne list             |  list all ipsec tunnels                                 |
| diagnose sys sdwan health-check  | Health Check und SLA status per link                    |
| diagnose sys sdwan service4      | Check link preference chosen by SD-WAN                  |
| diagnose sys sdwan advpn-session | Check advpn 2.0 path selection outcome (spoke to spoke) |
| diagnose firewall proute list    | Show policy routes (incl. sd-wan rules)                 |



### Links



{% embed url="https://community.fortinet.com/t5/FortiGate/Technical-Tip-Fortinet-SD-WAN-Remote-SLAs/ta-p/375338" %}
Remote SLA Checks
{% endembed %}
