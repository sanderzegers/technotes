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



Labeling vs Marking

| Labeling 💻                                                  | Marking 👨                        |
| ------------------------------------------------------------ | --------------------------------- |
| System-readable                                              | Human-readable                    |
| subjects and objects represented by internal data structures | human-readable form               |
| enables system-based enforcement                             | enables process-based enforcement |

System-readabilty: QR codes, RF Tags, Barcodes, GPS Tages, Metadata

## Data Classification Policy

|            |                                                                                                          |                                                                                                                                                                                                                                                                               |
| ---------- | -------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| standards  | Establish mandatory, organization-wide **rules** that translate the policy into measurable requirements. | <p>• Definition of each classification label (e.g., <em>Public, Internal, Confidential, Restricted</em>).<br>• Handling requirements per label (encryption strength, access-control mechanisms, minimum logging).<br>• Approved cryptographic algorithms and key lengths.</p> |
| procedures | Steps for carrying out tasks and policies                                                                | step-by-step instructions, workflows, very granular                                                                                                                                                                                                                           |
| baselines  | minimum security configuration, that every asset in given class must meet                                | technical minimums                                                                                                                                                                                                                                                            |
| guidelines | recommended best practice, non-compulsory                                                                | should and may statements                                                                                                                                                                                                                                                     |



hierarchy: **Policy → Standards → Procedures → Baselines → Guidelines**

**baselines are mandatory** like standards but focus on configurations, not behaviors

#### How They All Work Together

1. **Policy** says: “All sensitive information must be classified and protected according to business impact.”
2.  **Standards** turn that mandate into hard requirements:

    “Confidential data must be encrypted at rest with AES-256”.
3. **Procedures** tell an administrator exactly how to enable BitLocker with AES-256 on a specific build.
4. **Baselines** guarantee every server is locked down to an agreed-upon floor of controls.
5. **Guidelines** nudge users toward better-than-minimum behavior without forcing it.

Classification Levels

| Military Sector            | Private Sector       |   |
| -------------------------- | -------------------- | - |
| Top Secret                 | Sensitive            |   |
| Secret                     | Confidential         |   |
| Confidential               | Private              |   |
| Sensitive but unclassified | Company restricted   |   |
| Sensitive but unclassified | Company confidential |   |
| Unclassified               | Public               |   |

## Data roles

* Data Owner
  * person responsible for classifying, labeling and protecting data
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
  * day-to-day repsonsibilities for protecting and storing data
  * Grants permission on daily basis
  * ensures compliance with data policy and data ownership guidelines
  * ensure accessiblity, main and monitor
  * data archive
  * data monitor
  * implement security controls
