---
icon: cassette-tape
---

# Domain 2: Asset Security

Classification: system of classes ordered according to value\
Categorization: The act of sotring into defined classifications

Classification examples:\
Top secret, senstive\
Financially sensitive\
Company restricted\
PII

## Labeling vs Marking

| Labeling 🔦                                                  | Marking 🖍️                       |
| ------------------------------------------------------------ | --------------------------------- |
| System-readable                                              | Human-readable                    |
| subjects and objects represented by internal data structures | human-readable form               |
| enables system-based enforcement                             | enables process-based enforcement |

System-readabilty: QR codes, RF Tags, Barcodes, GPS Tages, Metadata

## Data Classification Policy

|            |                                                                                                                                                     | examples                                                                                                                                                                                                                                                                      |
| ---------- | --------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| standards  | Establish <mark style="color:$primary;">mandatory</mark>, organization-wide **rules** that translate the policy into measurable requirements.       | <p>• Definition of each classification label (e.g., <em>Public, Internal, Confidential, Restricted</em>).<br>• Handling requirements per label (encryption strength, access-control mechanisms, minimum logging).<br>• Approved cryptographic algorithms and key lengths.</p> |
| procedures | Steps for carrying out tasks and policies                                                                                                           | step-by-step instructions, workflows, very granular                                                                                                                                                                                                                           |
| baselines  | <mark style="color:$primary;">minimum security configuration</mark>, that every asset in given class <mark style="color:$primary;">must</mark> meet | technical minimums                                                                                                                                                                                                                                                            |
| guidelines | <mark style="color:$primary;">recommended</mark> best practice, non-compulsory                                                                      | should and may statements                                                                                                                                                                                                                                                     |



hierarchy: **Policy → Standards → Procedures → Baselines → Guidelines**

**baselines are mandatory** like standards but focus on configurations, not behaviors

#### How They All Work Together

1. **Policy** says: “All sensitive information must be classified and protected according to business impact.”
2.  **Standards** turn that mandate into hard requirements:

    “Confidential data must be encrypted at rest with AES-256”.
3. **Procedures** tell an administrator exactly how to enable BitLocker with AES-256 on a specific build.
4. **Baselines** guarantee every server is locked down to an agreed-upon floor of controls.
5. **Guidelines** nudge users toward better-than-minimum behavior without forcing it.

## Classification Levels

<figure><img src=".gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

|         | Government   | Privat Sector              |
| ------- | ------------ | -------------------------- |
| Class 3 | Top secret   | Confidential / Proprietary |
| Class 2 | Secret       | Private                    |
| Class 1 | Confidential | Sensitive                  |
| Class 0 | Unclassified | Public                     |

## Data roles

* Data Owner
  * person responsible for classifying, labeling and protecting data
  * usually senior management
  * defines level of classification
  * defines controls for levels of classification
  * decide when to destroy
  * establishing rules for the appropriate used and protection of data
* System Owner
* responsible for the systems that process data
* develop security plan, identifying and implementing security controls
* Business and mission owner
  * own the processes and ensure that the systems provide values to the org
* Data Controller
  * decide what data to process and how to process
* Data Processor
  * often 3rd party entities that process data for an org at the direction of the data controller
  * systems used to process data
* Administrators
  * grant access to data based on guidelines provided by data owner
* Custodian
  * <mark style="color:$primary;">day-to-day</mark> responsibilities for protecting and storing data
  * Grants permission on daily basis
  * ensures compliance with data policy and data ownership guidelines
  * ensure accessibility, main and monitor
  * data archive
  * data monitor
  * implement security controls



## Data Lifecycle

* Create
  * by systems
  * by users
* Store
  * classified before storage!
  * ideally encrypted at rest
* Use
  * security controls based on classification
* Share
  * transit over network
  * ideally encrypted
* Archive
  * sometimes laws or regulations require retention of data
* Destroy
  * neither readable nor recoverable
    * crypto-shredding

## Data destruction Methods

|                        |                                                                                                                   |                                    |
| ---------------------- | ----------------------------------------------------------------------------------------------------------------- | ---------------------------------- |
| Erasing / Deleting     | <mark style="color:$primary;">delete operation</mark> on file.                                                    | data is recoverable                |
| Clearing (overwriting) | <p>preparing media for reuse, data cannot be recovered using traditional recovery tools<br>one-time zero pass</p> |                                    |
| Purging                | <p>more intense form of clearing<br>multiple overwrite passes</p>                                                 | for use in less secure environment |
| Degaussing             | create strong magnetic filed that erases data on some media                                                       |                                    |
| Destruction            | final stage in the media lifecycle. most secure method                                                            | most secure method                 |
|                        |                                                                                                                   |                                    |

Don't keep data longer around than necessary -> Data retention. Can cause legal issues

## Scoping and Tailoring

customize security controls to fit the specific needs of your Org:\
repeating process, must be cost-effective for your business

Scoping

* decide where (or whether) the baseline control applies
* Defining the <mark style="color:$primary;">boundaries</mark> and <mark style="color:$primary;">assets</mark> that security controls will <mark style="color:$primary;">apply to</mark>
* part of the tailoring process
* review list of baseline security and privacy controls and select only those security and privacy controls that apply to the IT systems you're trying to protect

Tailoring

* adjusting how the chosen control will be implemented
* Adapting security controls to <mark style="color:$primary;">address the specific threats and vulnerabilities of your environment</mark>
* modifying the list of security controls within a baseline to align with the orgs missions



Examples scoping

Remote Maintenance. Out of scope for air-gapped devices\
Cryptographic protection. public data is left unencrypted

Examples tailoring

specify parameter values: org reviews all privileged accounts at least every 30 days.\
select or drop control enhancements: Automated correlation/analysis is added for high-impact system, while AU-6(3) non-mandatory alerts is not selected because 24x7 SOC coverage isn't feasible



## GPDR Terminalogy

| Term                              | Simple definition                                                                                                                                                  |
| --------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Data processing**               | Any operation on personal data—collect, store, use, share, or delete (automated or not).                                                                           |
| **Data controller**               | The entity that decides **why** and **how** personal data is processed.                                                                                            |
| **Data transfer (international)** | Moving personal data outside the EEA (or to an international organization); permitted only if GDPR transfer rules are met (e.g., adequacy decision or safeguards). |

Reducing GPDR exposure:

* **Anonymization**: removing all relevant data so that is impossible to identify original subject or person.
  * GPDR is no longer relevant
* **Pseudo Anonymization**: use pseudonyms or aliases.
  * less stringent requirements





