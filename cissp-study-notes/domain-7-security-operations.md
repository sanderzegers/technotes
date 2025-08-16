---
icon: user-helmet-safety
---

# Domain 7: Security Operations

## Investigations

Securing the scene

* sealing off access
* taking photos
* documenting location of evidence
* avoid touching anything

investigations should stand up to scrutiny and cross-examination

once evidence has been contaminated, it can't be decontaminated



### Evidence collecting

#### Evidence Sources

|                         |                                                                    |   |
| ----------------------- | ------------------------------------------------------------------ | - |
| Oral/Written statements | statements given to police, investigators or as testimony in court |   |
| Written documents       | Handwritten, typed, letters, checks, wills, contracts, etc         |   |
| Computer systems        | Whole system, including peripherals                                |   |
| Visual/Audio            | phtos, videos, surveillance footage                                |   |

#### Type of evidence

|                         |                                                                                                                                         |   |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------------- | - |
| Real evidence           | tangible physical objects (hard disks, USB drives, etc)                                                                                 |   |
| Direct evidence         | requires no inference. proves a fact being discussed: eyewitness, confessions                                                           |   |
| Circumstantial evidence | aka indirect evidence. ex. witness testifying that the defendant was near the computer storage area after it was broken into            |   |
| Corroborative evidence  | supports facts or elements of the case, not a fact on its own. Confirming and strengthening other facts                                 |   |
| Hearsay evidence        | testimony from witnesses who were not present.                                                                                          |   |
| Best evidence rule      | original evidence rather than a copy or duplicate should be entered                                                                     |   |
| Secondary evidence      | reproduction or substitute of an orignal document or item of proof. In cases where original evidence no longer exist, it may be allowed |   |
| Opinions                | Expert and non-expert                                                                                                                   |   |

### Evidence admissibility

|                      |                                                                    |   |
| -------------------- | ------------------------------------------------------------------ | - |
| real evidence        | consists of actual objects, that can be brought into the courtroom |   |
| documentary evidence | consists of written documents                                      |   |
| testimonial evidence | verbal or written statements by witnesses                          |   |

### Evidence admissible in a court or law

* evidence msut be relevant
* fact must be material to the case
* evidence must be competent or legally collected

### MOM: Motive Opportunity Means

serves as a guide when conducting an investigation.&#x20;

* What might have motivated the suspect?
* Did suspect have the opportunity to perpetrate the crime
* Did the suspect have means (die Mittel)



### Locard's Exchange Principle

with every crime, something is taken and left behind

fingerprints, dna



Digital/Computer Forensics

Live evidence: RAM, Cache, Buffers

forensic copy: bit-for-bit copy of a digital media source



## Mobile device forensics

* is hard!
* manufacturers frequently change OS structure, file structure, services
* No single method or tool
* hibernation and suspension of apps
* extensive new training for examiners



## Reporting and Documentation

* document every step
* most relevant evidence should be documented for the sake of use by all relevant stakeholders:
  * Prosecution/Defense
  * Judge/Jury
  * Regulators
  * Investors
  * Insurers

## Artifacts

* remnants of a breach or attempted breach
* breadcrumbs can potentially lead back to an intruder
* can be found on
  * computer systems
  * web browsers
  * disk storage

Examples:

* IPs
* hashes
* file names/types
* reg keys
* URLS



Chain of custody

* control of evidence to maintain integrity for the sake of presentation in court

very important: focused on having control of the evidence

* who collected and handled the evidence
* when
* where

protect evidence from tampering, contamination, corruption

tab, bag and carry



Five rules of Evidence

* authentic
  * evidence is not fabricated or planted
* accurate
  * evidence ahs not been changed or modified (integrity)
* complete
  * all parts present. whether the support or fail to support the case
* convincing or reliable
  * anybody should understand. High degree of veracity, high degree of truth. Nontechnical people must be able to understand
* admissible
  * evidence is accepted as part of a case and allowed into the court.



Types of investigations

|                | Overview                                                                                                                                                       | Who drives investigation?                                    |
| -------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------ |
| Criminal       | crimes, often accompanying legal punishment. Can lead to time in jail, and criminal record. Conducted by law enforcement at the local, state and federal level | Primarily law enforcement with support from the organization |
| Civil          | disputed between individuals or organizations. Fines or monetary penatly                                                                                       | Organizations, individuals and their attorneys               |
| Regulatory     | deals with violations of regulated activities                                                                                                                  | Associated regulatory body                                   |
| Administrative | internal violations of organizational policies and incidents identified by an org.                                                                             | The organization (managers, hr, compliance, security staff)  |



## Logging and monitoring

SIEM: Security Information and Event Management

SIEM capabilities:

* Aggregation
* Normalization
* Correlation
* Secure Storage
* Analysis
* Reporting



## Threat intelligence / Threat feeds

* subscription
* threat research and analysis and emerging threat trends



**UEBA**: User and Entity Behavior Analytics

* entity behavior is collected and input into a threat model
* typically included with SIEM solutions
* monitors the behavior and patterns of users and entities, logs and correlates the underlying data, triggers alerts
* <mark style="color:$primary;">Baseline</mark>
* Machine learning
* behavioral shifts and anomalies



continues monitoring

* Define
* Establish
* Implement
* Analyze/Report
* Respond
* Review/update



Reasons:

* threat environment is constantly changing
* new vulnerabilities
* assets in the org are changing
* new monitoring rules
* balance between false-positives and false-negatives





## Security Orchestration, Automation and Response (SOAR)

* take input from SIEM, user submission, manual input and apply rules and workflows

three key areas:

* Threat and vulnerability management
* Incident response
* Security operations automation



## Configuration management

asset management lifecycle:

* plan
* request
* procure
* receive
* manage
* retire



## Configuration management

integral part of secure provisioning

proper configuration of a device at the time of deployment

Policies, Standards, Baselines, and procedures inform configuraiton management

Hardening should be considered as part of the config mangement process

automated provisioning tools can help to ensure consistency



Device configuration should be documented and reviewed on a periodic basis.

* identify assets to keep under control
* configure assets
* document config
* verify config



## Foundational security operations concepts

* need to know
  * restrict users knowledge (access to data) required to perform their role
* least privilege
  * restrict user's actions/privileges to only those required
  * limit the scope if account gets compromised
* separations of duties and responsibilities (sod)
  * prevents collusion
* job rotation
  * employees rotate to different jobs
  * prevents collusion
* privileged account management
* SLAs



## Protecting media

MTBF important criterion when evaluating storage media, especially for valuable or sensitive information

Data is one of the most important assets

* paper
* microforms (microfilm)
* magnetic
* flash memory
* optical

processes must be in place that constantly move data to new nmedia.

file fomrats should be updated in order to maintain compatiblity

protection of data must be updated to reflect current cryptography standards

media type considerations:

* confidentiality
  * crypto safe for the next 10-20years?
* access speeds
* portability
* durability&#x20;
* media format
* data format



Hardware and Software asset management: Following items must be present:

* Asset management life cycle
* Inventories
* Patching
* Software licensing
* secure configuration



Conduct incident management

Incident Response

event = observable occurrence of something

incident = an adverse event

### Incident response process

DRMRRRL (Drumroll)



* Detection
  * monitor and identify potential incidents through alerts and user reports
* Response (active IR team)
  * activating the incident response team and coordinating initial response
* Mitigation (containment)
  * containing the incident and taking steps to limit its impact
* Reporting
  * notifying appropriate parties internal and external (law, vendors, customers)
* Recovery
  * restoring affected systems and services to normal operations
* Remediation (=prevention)
  * identifying and mitigating vulnerabilities that let to the incident
* Lessons learned
  * documenting the incident and identifying improvements



Goals:

* reduce impact
* maintain or restore business continuity
* defend against further attacks



Detection tools

* IPS/IDS
* DLP
* Anti-Malware
* SIEM
* Admin review
* Motion sensors
* CCTV
* Guards



Incidents examples:

* malware
* hacker attack
* insider attack
* employee error
* System Error
* workplace injury





Operate and maintain detective and preventive measures

|              |                                                                          |   |
| ------------ | ------------------------------------------------------------------------ | - |
| Virus        | triggered by the user somehow                                            |   |
| Worm         | self-propagate, by exploiting vulnerability                              |   |
| Companion    | attaches itself to legitimate programs                                   |   |
| Multipartite | spreads in different ways. USB and network                               |   |
| Polymorphic  | change code to evade detection                                           |   |
| Trojan       | looks harmless, but contains malicious code                              |   |
| Logic bomb   | Will execute based on some logic. eg. check HR DB if I'm still employee  |   |
| Data diddler | makes very small changes over a long period of time, to evade detection. |   |
|              |                                                                          |   |



Anti-malware

Most effective anti-malware is user training and awareness.



Change management

change management ensures that costs and benefits of changes are analyzed and changes are made in a controlled manner to reduce risks

CAB: Change Advisory Boards

|                      |                                                                              |
| -------------------- | ---------------------------------------------------------------------------- |
| Change request       | Can come from any part of the org                                            |
| Assess Impact        | <p>impact of potential change must be assessed</p><p></p>                    |
| Approval             | level of review should match criticiallity of systems and impact on business |
| Build and Test       | Ideal test environment                                                       |
| Notification         | notify stakeholder                                                           |
| Implement            | implement change                                                             |
| Validation           | inform senior mangement and stakeholder                                      |
| Version and Baseline | make sure documentation is complete                                          |



Failure modes

3 modes:

* fail-safe
* fail-soft (fail-open)
* fail-secure (fail-closed)



Fail-Soft / Fail-Open

* fail into state of less security, firewall allow all traffic.&#x20;

Fail-Secure / Fail-Close

* fail into state of same or greater security.

Fail-Safe

* fail into state that prioritizes the safety of people
  * Doors unlock automatically





Backup Storage Strategies

* drive by organizational goals and ojbectes
  * typically focus on backup and restore time and storage needs
* archive bit: metadata that indicates the status of a backup relative to a given backup strategy
* incremental backup: changes since last incremental backup
* differential backup: changes since last full backup



Archive Bit:&#x20;

* Stored in the metadata of a file. (Windows FS). Cleared when File was archived/backed up.
* Reset every time a file is written



RAID: Redundant Array of independent disks



Clustering: load balancing

Redundancy: primary and secondary standby system.



## Recovery Site Strategies

parameters: people, data, infrastructure and cost

Geographically remote and geographic disparity

Cold Site < Warm Site < Hot Site < Redundant site

**Cold site**

* shell of a building. heating ventilation cooling&#x20;

**Warm Site**

* basic equipment is installed; rack, cables are run.

**Hot Site**

* everything is ready to go except people and data.
* Servers, networks, all in place. Waiting for data to be restored and people move over

**Mobile site**

* form of hot site
* site on wheels
* mini data center
* government for hurricanes or other disasters

**Redundant** site

* everything is in place and working
* same cost as primary site.

**Service Bureau**&#x20;

* company that leases computer time.&#x20;
* Owns large server farms and often fields of workstation. May be onsite or remote



### Geographically remote / geographically disparity

internal sites belong to company, external sites to 3rd party

reciprocal agreements

* two companies support each other if their sites goes down
* quite rare

resource capacity agreements

* agreements between orgs and vendors to ensure they can secure the resources

multiple processing sites

* credit card: process transaction at multiple sites simultaneously&#x20;



## Disaster Recovery Solutions

**RPO**: Recovery <mark style="color:$primary;">Point</mark> Objective

* maximum amount of **data** you can afford to lose
* expressed as length of time between last good copy of your data

**RTO**: Recovery <mark style="color:$primary;">Time</mark> Objective

* maximum tolerable **downtime**
* how long it takes to move from time of disaster to the time of operating at a defined service level
* not necessarily back to business, some level of service that allows business to move forward



**BIA**: Business Impact Analyses&#x20;

* process that helps an org identify its most critical functions, services assets systems and processes&#x20;



Todo: Read more about BCM, BCP and DRP

BCM, BCP,  and DRP

**BCM**: Business continuity management

* provides structure for BCP and DRP
* management system
* programme owns standards, budgets and audits



**BCP**: Business Continuity Planning

* documented, executable plan
* keep critical business process running
  * often tied with SLA
* survival of the business. Effective response, strategic

**COOP**: Continuity of Operations Plan

* plan for continuing to do business until the IT infrastructure can be restored

**DRP**: Disaster Recovery Plan

* plan for recovering IT infrastructure and data

**BRP**: Business Resumption Plan

* plan to move from disaster recovery site back to normal business

## Time measurements

**MTD**: Maximum tolerable Downtime\
**MAD**: Maximum allowed Downtime

* after this time window, operations might cease to operate
* RTO should never exceed MTD

**WRT**; Work Recovery Time

* time needed to verify the intergrity of stems and data
* also component of MTD



## BIA: Business Impact Analysis

BIA process:

* most critical/essential business functions, processes and systems
* most important step in BCP
* potential impact of a disaster
* key measurements of time (RPO, RTO, WRT and MTD) for each critical function, processes and systems

Todo: Read more about BIA

The BIA Process

|                                                               |                                                                                                                                         |   |
| ------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------- | - |
| Determine mission/business processes and recovery criticality | <ul><li>indentify business processes</li><li>determine impact of disruption</li><li>outage impacts</li><li>estimated downtime</li></ul> |   |
| Identify resource requirements                                |                                                                                                                                         |   |
| Identify recovery priorities for systems resources            |                                                                                                                                         |   |
|                                                               |                                                                                                                                         |   |



## Disaster Response Process

* disaster is declared when MTD is going to be exceeded
* should include all personnel and resources necessary to quickly respond to the situation and restore normal operation
* Disaster Response team should include stakeholders from throught the ORG
* communications is critical



even -> incident -> disaster

* depends on asset, how operations and processes might be impacted
* risk to value
* declaring disaster done by CEO or Business Continuity Board
* activating BCP also done by CEO or Business Continuity Board

Incident repsonse process should be followed.

* should be tested at least once per year



communication during disaster:

* internal: critical. senior mgmt, board members, business owners, legal, hr and communications
* external: regulators, law enforcement, customers, media and others



Restoration Order

* BIA determines restoration order when recovering systems
* dependency charts and mapping can help inform system restoration order
* most critical systems should be brought online at recovery site
* start with least critical system at primary site to make sure primary is working correctly again

Dependency chart:

* load-balancer -> front end server -> backendserver





## Test DRP

BCP and DRP

* testing is critical component&#x20;
* DRP test include: read-through/checklist, walk through, simulation, parallel, full-interruption/full-scale
* full-interruption test should only be performed after management approval and other tests were successful



5 types of disaster recovery plans:

* **read-through test**
* **structured walk through test**
  * all of key stakeholders convene in a conference room
  * walk through DRP together
  * paper-based
  * aka table-top exercise
* **simulation test**
  * paper-based
  * facilitator moderates a scenario that requires the stakeholder to respond
  * facilitator can throw curve balls in the situation
* **parallel test**
  * use of backup systems, not anything in production
  * relocating personnel to the alternate recovery site
* **full-interruption or full-scale test**
  * production systems are impacted



### Related Terms

Recovery Team

* get critical business functions running at the alternate site

Salvage Team

* return the primary site to normal processing conditions

Electronic Vaulting

* transfer database backup to a remote site

Remote journal

* only journal or transaction logs are copied

mirroring

* mirror db to other site



## Goals of BCM

three goals:

* safety of people
* minimization of damage
* survival of business



Employee under Duress: threats violence, constrains to do something against their will or better judgment.

* use of code words



## Information lifecycle

Creation

* can be created by users or systems

Classification

* to ensure it's handled properly

Storage

* protected by adequate security controls based on classification

Usage

* data is in use, or transferred over the network

Archive

* comply with laws or regulations

Destruction

* destroy in a way it's not readable



## Audit trail



## Auditing & Due Care

Security audits and effectiveness reviews are key elements in displaying due care.



Alternatives to confiscating evidence

* Voluntary surrender
* subpoena
  * compel the subject to surrender the evidence
* search warrant
  * confiscate evidence without giving the subject an opportunity to alter it

data retention should be defined in security policies



Disaster Recovery Stakeholders

Disaster recovery team

* reliable real-time channels

Management

* periodic updates on progress and potential impacts on normal operation

Wider company

* general awareness, ahead of scheduled tests, pre- and post announcements

Customers/Clients

* proactive controlled communications
* ETA for resolutions

Partners

* inform parnters with relevant timeles
* agree on coordination channels

Regulators

* certain industries have reporting requirements
* advance notification or post-test reports
* include compliance experts in plan creation and testing





