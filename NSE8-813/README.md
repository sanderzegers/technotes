# Exam certification overview

## Topics

Source: [https://nseti-pdfs.s3.us-west-2.amazonaws.com/NSE8\_Assets/FCX\_Certification\_Public-Handbook.pdf](https://nseti-pdfs.s3.us-west-2.amazonaws.com/NSE8_Assets/FCX_Certification_Public-Handbook.pdf)



1. Security Architecture
   * Demonstrate knowledge of FortiGate Network Security products
     * Chassis solutions 6000/7000 modules and architecture
     * Correct hardware production selection based on design
   * Demonstrate knowledge of Fortinet Security Fabric Solution deployments
     * FortiMail
     * FortiSandbox
     * Traditional networks and hybrid/cloud/multi-cloud networks
   * Logging and management protocols used by Fortinet, and required network architecture for resiliency
     * Demonstrate knowledge of Fortinet high-availability solutions
     * Core products
     * Types of the HA solutions
     * HA and Cloud deployments
     * Optimization
2. Infrastructure
   * Demonstrate knowledge of FortiGate operation modes
     * Transparent Mode and Layer-2 Traffic
     * VDOM and VDOM links
   * Demonstrate knowledge of FortiGate hardware technology
     * NP6/NP7/nTurbo/CP9/SoC4 acceleration and acceleration concepts
     * Hyperscale requirements, operation, limitations
     * Traffic Flows during acceleration and offloading
     * Describe and design hardware accelerated networks with FortiGate devices
     * FortiGate chassis/module architecture
     * Life of packet
     * Hardware offloading
   * Demonstrate knowledge of non-FortiGate hardware technology
     * Hardware v virtual
     * FAZ, SIEM
   * Demonstrate knowledge of Fortinet solutions for cloud security
     * Private cloud
     * Public cloud
     * SAAS
     * SASE
3. Networking
   * Demonstrate knowledge of advanced routing and networking technologies
     * Static Routing
     * Dynamic Routing (OSPF/BGP)
     * Routing and high availability concepts
     * Asymmetric Routing
     * Secure SD-WAN Routing
     * Policy Routing
     * Multi-cast routing
     * Routing control
     * NAT
       * Dual-bidirectional NAT between two address domains
       * Interpret NAT information presented in Session table output
     * IPv6
       * NAT46 & NAT 64, SLAAC, DHCPv6, DNSv6
     * Traffic shaping
       * Interface-based shaping configuration
       * Effects on hardware acceleration
     * Virtual wire pairs
       * VWP with VLAN tags
   * Demonstrate knowledge of advanced VPN design methodologies
     * SSL VPN / IPSEC
     * Aggregate VPN
     * ADVPN
     * VXLAN over IPSEC
     * GRE
     * IKEv1 vs IKEv2 differences
   * Demonstrate knowledge of Fortinet access solutions advanced configurations and features
     * FortiSwitch advanced configurations
       * MCLAG
     * FortiAP advanced configurations
       * Remote tunneling
     * Advanced use cases of FortiExtender (IPSEC VPN, VLAN mode)
       * IPSEC VPN
       * VLAN mode
     * FortiOS access control features
       * Control Policy
       * Device Profiling
       * DHCP Option 82
       * FortiNAC configuration
       * Remediation Policy
   * Demonstrate knowledge of how to integrate Fortinet access solutions
     * Advanced authentication for access layer
       * FortiAP radius based dynamic vlan
       * RADIUS based dynamic VLAN
     * FortiLink advanced configurations
       * Quarantine NAC vlans
       * FortiLink over L3
     * Centralized management of access products from FortiManager
       * Design Fortinet access layer solutions
         * Wireless planning
         * Switch stack design
         * ZTNA solutions
       * Fortinet Security Fabric and integrated management of Firewall, access, and ATP products
   * Demonstrate knowledge of application delivery
     * Load balancing
     * Health checks
4. Secure SD-WAN
   * Demonstrate knowledge of SD-WAN advanced architecture and design
     * Design and implement a full featured SD-WAN solution with dynamic routing
     * Local traffic routing with SD-WAN
     * Understanding SD-WAN rules and failover
   * Demonstrate knowledge of SD-WAN advanced features
     * Azure vWAN
     * ADVPN design and requirements
     * Packet duplication and aggregate tunnels
     * Network overlays
   * Demonstrate knowledge of SD-WAN troubleshooting
     * Session failover with NAT
     * Session route change with max bandwidth method
     * Shortcut tunnels and BGP
5. Security Solutions
   * Demonstrate knowledge of Fortinet application security solutions
     * Operation and deployment modes
     * Designing resilient solutions
     * Advanced security inspection
     * FortiGuard services for enhanced Fortinet solutions
     * Troubleshooting application security issues
   * Demonstrate knowledge of Fortinet network security solutions
     * Inspection modes
     * Security profiles
     * Troubleshooting FortiOS security features
     * FortiGuard services for FortiOS security services
     * VoIP
       * VoIP ALG / proxy
       * SIP kernel-helper
       * Flow SIP
     * HTTP/2
       * SSL inspection with HTTP/2
   * Demonstrate knowledge of authentication mechanisms
     * Implement SAML authentication
     * Integrate external authentication using Radius / LDAP
     * Configuring Fortinet product authentication using FortiAuthenticator
     * Authentication using VSAs with Radius for automated roles / profiles
     * Two factor authentication using certificates and tokens'
     * Fortinet FSSO using collectors and FortiAuthenticator
     * Integrate with AD certificate services
     * RBAC, authentication and certificate management solutions with Fortinet Management products
6. Security Operations
   * Demonstrate knowledge of Fortinet SOC solution
     * Integrate Fortinet solutions for advanced threat protection
     * Security incident handling
     * Security incident enrichment
     * Threat analysis and incident response
     * Automated remediation
     * Fortinet management and logging tools
   * Demonstrate knowledge of Fortinet endpoint solutions
     * Network admission control solution
     * Device On-boarding using various methods
     * FCT Client Profile
     * VPN Profile Management
     * FortiClient EMS installation package managing
     * EMS on net / off net
     * ZTNA Policy / configuration (EMS/FCT/FG/FAC)
     * Endpoint protection (Client/Guest)
     * Quarantine functions on both LAN/WLAN
     * EDR - Playbooks / Exceptions
       * Automation
     * Demonstrate knowledge of Fortinet Automation tools, solutions, and integrations
       * Automation Stiches
       * Understand Fabric connectors
       * Zero Touch Configuration/Zero Touch Provisioning
       * Automated Response Systems (SOAR/Handlers)
       * FortiSIEM log automation triggers
     * Demonstrate knowledge of Fortinet build-in scripting capabilities
       * FortiManager CLI/TCL Scripting
       * FMG CLI Template + Variables
       * FortiGate AutoScript
     * Demonstrate knowledge of Fortinet API configuration and usage
       * FortiGate webhook triggers
       * API Integration within the Security Fabric
       * Understand principles of API usage (including required config)
       * Solutions for rollout and management of large scale FortiGate networks (Fortinet or 3rd party management tools)



## Products

| Product                | Course Material Only | Handbook | Certification |
| ---------------------- | -------------------- | -------- | ------------- |
| FortiGate 7.x          |                      | Yes      | Yes           |
| FortiAnalyzer 7.x      |                      | Yes      |               |
| FortiAuthenticator 6.x |                      |          |               |
| FortiManager 7.x       |                      | Yes      |               |
| FortiSandbox 4.x       |                      |          |               |
| FortiADC 7.x           |                      |          |               |
| FortiWeb 7.x           |                      |          |               |
| FortiMail 7.x          |                      |          |               |
| FortiClient 7.x        |                      |          |               |
| FortiClientEMS 7.x     |                      |          |               |
| FortiSwitch 7.x        |                      |          |               |
| FortiAP 7.x            |                      |          |               |
| FortiNAC 9.x           |                      |          | Yes           |
| FortiExtender 7.x      | Yes                  |          |               |
| FortiDDoS 5.x          | Yes                  |          |               |
| FortiSIEM 6.x          |                      |          |               |
| FortiEDR 5.x           |                      |          |               |
| FortiSOAR 7.x          |                      |          |               |

## Trainings

{% embed url="https://www.fortinet.com/content/dam/maindam/PUBLIC/02_MARKETING/02_Collateral/Brochures/nse-training-brochure.pdf" %}

| Training                     | Exams                                                                                                                                                       |
| ---------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| FortiSASE Core Administrator | FortiSASE & SD-WAN Core Administrator                                                                                                                       |
| SD-WAN Core Administrator    | FortiSASE & SD-WAN Core Administrator                                                                                                                       |
|                              | [**Fortinet NSE 4 - FortiOS 7.6 Administrator**](https://training.fortinet.com/local/staticpage/view.php?page=fortios_administrator_exam)                   |
|                              | [Fortinet NSE 6 - SD-WAN Enterprise Administrator](https://training.fortinet.com/local/staticpage/view.php?page=sd-wan_enterprise_administrator_exam)       |
|                              | [Fortinet NSE 7 - FortiSASE Enterprise Administrator](https://training.fortinet.com/local/staticpage/view.php?page=fortisase_enterprise_administrator_exam) |
|                              | [Fortinet NSE 7 - Enterprise Firewall Administrator](https://training.fortinet.com/local/staticpage/view.php?page=enterprise_firewall_administrator_exam)   |
|                              | [**Fortinet NSE 6 - OT Security 7.6 Architect**](https://training.fortinet.com/local/staticpage/view.php?page=ot_security_architect_exam) **/** coming      |

FortiOS Administrator

