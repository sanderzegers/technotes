---
icon: stethoscope
---

# Domain 6: Security Assessment & Testing

The purpose of security assessment and testing is to ensure that security requirements/controls are defined, tested and operating effectively.\
ongoing process!

* vulnerability assessments
  * search for known vulnerabilities
* penetration tests, software testing
  * same tools but supplements them with attack techniques, and vulnerability exploitation
* audits
* security management tasks

Every organization should have a security assessment and testing program defined and operational



**Validation**: Are we building the right product?

**Verification** follow validation: Are we building the product correctly



Validation happens prior to an application or product being built.

The 3 Cs: (co-co-co)

| C                | What it means                                                              | Why it matters                                                                                                                    |
| ---------------- | -------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| **Correctness**  | Every stated requirement is satisfied by the specification/implementation. | A system that “works as written” but not as _needed_ still fails the business. Correctness links the build to the business need.  |
| **Completeness** | Nothing essential is missing from the requirement set.                     | Missing requirements surface late as costly re-work or security gaps (e.g., forgotten audit trail).                               |
| **Consistency**  | No requirement contradicts another; terminology and rules are uniform.     | Inconsistent specs lead to undefined behaviour and exploitable edge cases.                                                        |



Security assessment and testing: provide assurance regarding the architecture, application or system being tested

Effort to invest in testing should be proportionate to the value the application or system represents to the org



| Internal Audit                           | External Audit                                                               | Third-Party Audit                                                    |
| ---------------------------------------- | ---------------------------------------------------------------------------- | -------------------------------------------------------------------- |
| Testing conducted by internal to the org | Either - somebody internal to the org examining an external service provider | Three parties are involved: customer, vendor, independent audit firm |
|                                          | Or Org asking sombedoy external to audit internal app or system              |                                                                      |

Location: On-premise, cloud, hybrid

Role of security professional:

* Identify Risk
* Advise test processes to ensure risks are appropriately evaluated
* Provide advice and support to stake holders



## Security Control Testing

Software Testing: unit testing, interface testing, integration testing, system testing

Testing is required during every stage of a systems' life cycle!

Software Testing Stages

| Development Stage | Security Testing                                                                                                                                           |
| ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Planning          | Verify requirements have been accurately captures                                                                                                          |
| Design            | <p>advice on what type of controls the system should have for CIA.<br>Test to confirm all required controls are in systems design, architecture design</p> |
| Develop           | unit testing, integration testing, system testing, acceptance testing, vulnerability assessments                                                           |
| Deploy            | usability testing, performance testing, reviewing logs for errors and anomalies, vulnerability assessments                                                 |
| Operate           | Config management review. Vulnerability management and log analysis                                                                                        |
| Retire            | Test data has been migrated to new system. safely disposing from old one                                                                                   |

## Static vs dynamic testing

**SAST**: Static Application Security Testing.

* Source Code analysis

**DAST**: Dynamic Application Security Testing

* App is running
* black box testing, code not visible

## Fuzzer

**Fuzz Testing**

* chaos, randomness



* mutation (dumb fuzzers)
  * fliping bits or appending/replacing additional random input
* generation (intelligent fuzzers)
  * input generated based on understanding file format or prtocol



## Test Types

* positive testing
  * normal user input
* negative testing
  * focus on error / exception handling
  * expacted input errors
* Misuse testing
  * perspective on hacker/cracker



Equivalence Partitioning:

* Inputs are divided into gorups that exhibit the same behavior with test cases covering each partition

Boundary Value Analysis

* Focus on testing data the boundaries. Covering extreme ends of the input values

**Putting them together in practice**

1. **Start with** Equivalence Partitioning: carve the input space into “valid” and assorted “invalid” partitions.\
   &#xNAN;_&#x45;xample_ – Age field
   * &#x20;Valid: 1-120 • Invalid-low: ≤0 • Invalid-high: ≥121 • Invalid-type: non-numeric\*
2.  **Apply** Boundary Value Analysis **to every numeric or ordered partition**:

    • 0 / 1 / 2 and 119 / 120 / 121\*

This yields a tight, high-value test set:

```
CopyEditAge tests → {0, 1, 2, 119, 120, 121, “abc”}
```

Seven cases cover the entire domain instead of brute-forcing thousands.

|                           | Equivalence Partitioning                         | Boundary Value Analysis                       |
| ------------------------- | ------------------------------------------------ | --------------------------------------------- |
| **Goal**                  | Reduce combinatorial explosion                   | Catch edge-case faults                        |
| **Input picks**           | One _representative_ from each class             | Values _at and around_ every limit            |
| **Typical defects found** | Wrong accept/reject logic, data-type mishandling | Off-by-one, overflow, truncation, sign errors |
| **Mnemonic**              | **“Partition first, then probe the edges.”**     |                                               |



???

Decision Table Analysis

* Different input combinations and their corresponding system behavior/output are captured in a table

State-based Analysis

* Set of abstract staes that a unit of software can take are defined, and then test compare its actual sate of the expected state (useful for GUI testing and communication protocols)



Vulnerability testing: automatic\
Pentesting: manual, can take several days



Threat modeling methodologies:

* STRIDE (Spoofing, Tampering, Repudiation, Information disclosure, Denial of service, Elevation of privilege)
* PASTA (Process for Attack Simulation and Threat Analysis)



Penetration test steps:

* Reconnaissance (gather public info)
* Enumeration (enumerate through target (IPs, ports, etc)
* Vulnerability Analysis (Identify potential vulnerabilities)
* Execution (expoit)
* Document Findings (Report)



Blind testing: assessor is given little to no information.

Double-Blind testing: assessor is given little to no information + SECOPS Teams are informed about upcoming test. Usually only senior-management

Effective vulnerability management:

* asset inventory
  * value of each asset
  * identified owner for each asset
  * assigned classification and categorization for each asset
* vulnerability for each asset and remediation plans
* ongoing review and assessment

## CVS & CVSS

CVE: Common vulnerability & Exposure

* ensures that each vulnerability is only identified and recorded one time

CVSS: Common Vulnerability Scoring System

* Framework that uses mertric and characteristics to provide an average score of how severe a vulnerability is.
* 0 - 10
* CVSS rating added by Vendor, or added 1 hour after CVE gets published
  * Nist NVD always calculates own CVSS rating



## Security Management Oversight

Security Managers must perform activities to retain oversight over the infosec program

* log review
* account management review
* backup verification
* key performance and risk indicators
  * high-level view of security program effectiveness

## Log Review and Analysis

Log should:

* include what is relevant
* proactively reviewed
* time synced



Log review can be automated (SIEM) or manually

circular logging: overwrite old entries

clipping level: Log upon certain threshold is met (wrong password after 5 retries)



Operational testing:

* Test while system is operational
* Real User Monitoring
  * passive monitoring
  * monitors user interactions and activity
* Synthetic Performance Monitoring
  * making up transactions
  * scripted transcations. Monitor functionality, availabiliyt and response times



Regression Testing:

* process of verifying that previously tested and functional software still works after updates
* perform after enhancements or patches

Reports depends on reader (ceo, app team)

* objective pass/fail
* metric that metter



Compliance checks:

* integral and ongoing part of security control testing



Collect security process data

* key risk and perofrmance indicators help
  * goal setting
  * action planning
  * performance
  * review



SMART Metrics: Specific, Measurable, Achievable, Relevant, Timely

* describe datapoints that can be used for goal setting
* often used in context of employee performance and review

KPI: Key performance indicators

* backward looking
* indicate achievement of performance target

KRI: Key risk indicators

* forward looking
* exposure to operational risk



| Good SMART metric                                                                                                                                            | Bad metric                                                                                  | Aspect         |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------- | -------------- |
| “By **31 Dec 2025**, reduce the **median** time-to-remediate (_TTR_) for all externally exposed assets with a **CVSS Base ≥ 7.0** to **≤ 7 calendar days**.” | “Patch vulnerabilities as quickly as possible.”                                             | **Wording**    |
| ✔ Focuses on _which_ assets (externally exposed) and _which_ vulns (CVSS ≥ 7.0).                                                                             | ✖ “Quickly” and “vulnerabilities” are undefined.                                            | **Specific**   |
| ✔ Uses the metric “median TTR” and a threshold of 7 days.                                                                                                    | ✖ No quantifiable target or unit.                                                           | **Measurable** |
| ✔ Seven-day SLA is aggressive but realistic for critical/high findings when the organisation already averages \~10 days.                                     | ✖ Impossible to tell—no target to gauge feasibility.                                        | **Achievable** |
| ✔ Ties directly to reducing the window of exposure for the highest-risk flaws (a common board-level concern).                                                | ✖ May encourage chasing _all_ vulnerabilities equally, diluting effort away from real risk. | **Relevant**   |
| ✔ Deadline (31 Dec 2025) and a per-vuln remediation window (7 days).                                                                                         | ✖ Lacks any timeframe.                                                                      | **Time-bound** |

Example Areas for Metrics:

* Account Management
* Management review and approval
* Backup verification
* Training and Awareness
* Disaster recovery and Business continuity



Analyze test output and generate report

Security Assement and testing report should include steps related to:

* Remediation
* Exception handling
* Ethical disclosure

|                    |                                                                                                                                                         |   |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------- | - |
| Remediation        | Based upon assessments and testing, remediation steps for all identified vulnerabilities should be documented                                           |   |
| Exception Handling | Exceptions should be documented and reasoned. (ex. too much cost)                                                                                       |   |
| Ethical disclosure | it's important that newly discovered vulnerabilities be shared to the extent necessary to protect anybody who may be exposed the the same vulnerability |   |

## Internal and External Audit

Audit:

* internal
  * performed by an org internal staff
  * intended for management use
* external
  * performed by 3rd party (big 4)
  * generally intended for the organization's governing body and investors
*   3rd party

    * audits conducted by or on behalf of another organization



Internal audits involve internal employees focused on organizational processes

External Audits involve employees focusing on vendor processes

3rd party audits involve independent auditors focusing on vendor processes

Often times a hybrid approach is applied

Test tip: Assume audit is 3rd party unless question says otherwise



Audit plan:

* Define the audit objective
* define the audit scope
* conduct the audit
* refine the audit process

Audit process:

* Determining audit goals
* Involving the right business unit leaders
* Determe the audit scope
* Choose audit team
* plan the audit
* Conduct the audit
* Document audit results
* Communicate audit



SAS70 -> SSAE 16 -> SSAE 18

every SOC 1 / SOC 2 / SSAE-18 engagement is led and signed by an _independent_ CPA firm; the Big Four and dozens of specialist boutiques dominate the market.

| SOC family |                                                                                             | Primary audience                            | Common CISSP-relevant use                                 |
| ---------- | ------------------------------------------------------------------------------------------- | ------------------------------------------- | --------------------------------------------------------- |
| **SOC 1**  | financial reporting risks                                                                   | Financial auditors, CFOs                    | Outsourced payroll, claims processing.                    |
| **SOC 2**  | 5 trust principles: security, availabilty, confidentialiy, processing integrity and privacy | Security & compliance teams                 | Cloud hosting, SaaS platforms, managed security services. |
| **SOC 3**  | marketing tool                                                                              | Public marketing copy of SOC 2 (short-form) | Vendor due-diligence “logo” on websites.                  |

Type 1 report

* focus on design of controls at a point of time
* focus on paperwork (policies, procedures, baselines etc)
* evaluate whether a process is properly designed

Type 2 report

* examines design of a control
* **control effectiveness over a period of time**
* Checks entries in change requests for example.

SOC 2, Type 2 most desirable report for security professionals!!

Startups normally start with type 1 to identifiy missing controls, gaps, etc. The years after it will be Type 2 report.

#### How the pieces fit together in practice

1. **Engagement letter** – Service organisation hires an independent CPA firm.
2. **Planning & risk assessment** – Auditor maps the scope, including **subservice organisations** (e.g., a cloud IaaS provider) now mandatory under SSAE-18.
3. **Fieldwork**
   * **Type I:** test design at a point in time.
   * **Type II:** test design _and_ operating effectiveness over 6-12 months (log samples, change tickets, etc.).
4. **Opinion & report issuance** – CPA signs the SOC report; management’s assertion and the detailed control matrix are included.
5. **Reliance by others** –
   * **User auditors** reference the SOC 1 in their own financial-statement audit working papers.
   * **Security teams** ingest the SOC 2 into their vendor-risk platform to avoid repeat questionnaires.
   * **Regulators** (e.g., banking supervisors) accept a SOC 2 Type II as part of third-party-risk evidence.





## Audit Roles and Responsibilities

Audit roles:

* executive (senior) management
* audit commitee
* security officer
* compliance manager
* internal auditors
* external auditors



|                      |                                                                                                     |
| -------------------- | --------------------------------------------------------------------------------------------------- |
| Executive Management | Sets proper tone from top, promote audit process                                                    |
| Audit committee      | Members of Board/senior stakeholders to provider oversight                                          |
| Security Officer     | Advise on security related risks to be evaluated                                                    |
| Compliance Manager   | Ensure corporate compliance with applicable laws and regulations, professional standards,           |
| Internal auditors    | company employees who provide assurance that corporate internal controls are operating effectively  |
| External Auditors    | Provide an unbiased and indepdent audit report as they are independent of the entitty being audited |

