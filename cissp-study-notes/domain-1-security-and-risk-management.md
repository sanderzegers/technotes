---
icon: fire
---

# Domain 1: Security and Risk Management

***

#### 📜 (ISC)² Code of Ethics Preamble (Summary):

> **"The safety and welfare of society and the common good, the duty to our principals, and to each other requires that we adhere, and be seen to adhere, to the highest ethical standards of behavior."**



#### 🛡️ (ISC)² Code of Ethics Canons:

1. **Protect&#x20;**<mark style="color:$primary;">**society**</mark>**, the&#x20;**<mark style="color:$primary;">**common good**</mark>**, necessary&#x20;**<mark style="color:$primary;">**public trust and confidence**</mark>**, and the infrastructure.**
   * This means prioritizing the <mark style="color:$primary;">safety and well-being of the public</mark>, supporting critical infrastructure, and acting in ways that build trust in systems and services.
2. **Act&#x20;**<mark style="color:$primary;">**honorably**</mark>**,&#x20;**<mark style="color:$primary;">**honestly**</mark>**,&#x20;**<mark style="color:$primary;">**justly**</mark>**, responsibly, and legally.**
   * Maintain integrity in all professional and personal conduct. Avoid conflicts of interest and act in compliance with the law.
3. **Provide diligent and competent service to principals.**
   * "Principals" refers to <mark style="color:$primary;">those you serve</mark>, such as employers, clients, or the public. You must be competent in your work and act with due care.
4. **Advance and protect the profession.**
   * Support the growth, integrity, and reputation of the <mark style="color:$primary;">cybersecurity profession</mark>. This includes mentoring others, sharing knowledge responsibly, and not bringing the field into disrepute.



CIA:

* Confidentiality
* Integrity
* Availability

Five pillars = CIA triad +

* authenticity
  * Data is genuine and not spoofed
  * Verifying the identity of a subject or resource
* non-repudiation
  * Someone cannot deny their actions (sending messages, making transanctions)
  * provides undeniable proof
  * always includes authenticity

opposite of / failure of CIA = DAD:

* Disclosure
* Alternation
* Destruction



## Security Roles

|                       |             |                                                                                                                                  |
| --------------------- | ----------- | -------------------------------------------------------------------------------------------------------------------------------- |
| Senior Manager        | Owner       | ultimate authority on security, ensuring policy approval, resource allocation, and bearing final responsibility for its cusscess |
| Security Professional | Implementer | technical expert who design, implement, and enforce security solutions based on policy directives                                |
| Asset Owner           | Classifier  | High-level manager who determine the appropriate level of protection for assets                                                  |
| Custodian             | Enforcer    | carrying out the security measures outlined in the policy                                                                        |
| User                  |             | accessing secure system. Responsible for following security protocols                                                            |



**Governance**: act of governing or overseeing the process of directing something. It's about improving the company with processes, new services, improving margins, meeting compliance requirements etc.

Policies = "corporate law"

accountability vs responsibility



## Security Control Frameworks

Set of <mark style="color:$primary;">best practices and standards</mark> that define <mark style="color:$primary;">security controls</mark> organizations can implement to mitigate security risks.

**select, implement, assess, and improve** security controls to manage risk and meet business, legal, and compliance goals.

NIST, ISO, COBIT, ITIL

### ISO

|           |                                                                                                                                                  |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| ISO 27001 | <p>certifiable, standard that says what an org must have for an ISMS (policy, risk-based controls, governanace)<br>requirements + audit</p>      |
| ISO 27002 | best-practice guide how to implement security controls                                                                                           |
| ISO 22301 | certifiable, <mark style="color:$primary;">business continuity requirements</mark>. BIA, strategies, plans, RTO/RPO. -> continuity you can audit |
| ISO 28000 | certifiable, supply chain security management.                                                                                                   |
| ISO 31000 | enterprise risk management guidance. . high-level principles and process for managing risk across the enterprise                                 |

### NIST

NIST 800-53: Security and Privacy Controls for Information Systems and Organizations

CIS: cisecurity. provides OS, apps, hardware security configurations

NIST Risk Management Framework (RMF):

* mandatory requirements for federal agencies.

NIST Cybersecurity Framework (CSF)

* voluntary

### COBIT

Control Objectives for Information and Related Technology.&#x20;

* documented set of best IT security practices crafted by <mark style="color:$primary;">ISACA</mark>
* prescribes <mark style="color:$primary;">goals and requirements for security controls</mark> and encourage <mark style="color:$primary;">mapping of it security ideals to business objects.</mark>
* mainly intended for <mark style="color:$primary;">IT governance</mark> within large enterprises

Cobit phases:

1. Evaluate, Direct and Monitor
   1. provides governance
2. Align, Plan, and Organize
3. Build, Acquire and Implement
4. Deliver, Service, and Support
5. Monitor, Evaluate and Assess



## Due care vs Due diligence

* Due care is the <mark style="color:$primary;">responsible protection of assets</mark>
  * doing what a reasonable person would do in a given situation
  * implementation
  * operation
* Due diligence is the ability to <mark style="color:$primary;">prove due care</mark>
  * <mark style="color:$primary;">establish a plan, policy and process to protect interests of an organization</mark>
  * evaluation
  * think before you act



📊 Intellectual Property Comparison Table

<table data-header-hidden><thead><tr><th></th><th width="128"></th><th></th><th></th><th></th><th></th><th></th></tr></thead><tbody><tr><td><strong>Type</strong></td><td><strong>What it Protects</strong></td><td><strong>How it's Protected</strong></td><td><strong>Duration</strong></td><td><strong>Disclosure Required</strong></td><td><strong>Loss Risk</strong></td><td><strong>Examples</strong></td></tr><tr><td><strong>Trade Secret</strong></td><td>Confidential info (e.g., formulas, algorithms, methods)</td><td>Kept secret via internal controls, <mark style="color:$primary;">NDAs</mark></td><td><mark style="color:$primary;">Indefinite</mark> (as long as secret is kept)</td><td>No</td><td>If publicly disclosed or reverse-engineered</td><td>Coca-Cola formula, Google algorithm</td></tr><tr><td><strong>Patent</strong></td><td>Inventions, processes, and technical solutions</td><td>Government registration</td><td><mark style="color:$primary;">20 years</mark> (utility patent)</td><td>Yes (full public disclosure)</td><td>Expires after term, can be invalidated</td><td>iPhone hardware design, pharmaceutical drugs</td></tr><tr><td><strong>Copyright</strong></td><td>Original <mark style="color:$primary;">creative works</mark> (code, books, art, music)</td><td>Automatic upon creation (registration optional)</td><td><mark style="color:$primary;">Life of author + 70 years</mark> (U.S.)</td><td>Yes</td><td>Infringement, improper licensing</td><td>Software source code, novels, movies</td></tr><tr><td><strong>Trademark</strong></td><td>Brand names, logos, slogans</td><td><mark style="color:$primary;">Registration or common law use</mark> in commerce</td><td>Indefinite (with use &#x26; renewal)</td><td>Yes (public use/association)</td><td>Loss through non-use, dilution, or infringement</td><td>Nike swoosh, Apple logo, McDonald's slogan</td></tr></tbody></table>

4 licensing types:

* contractual
* shrink wrap
* click-through
* cloud services



Data Residency regulation: GDPR (General Data Protection Regulation)\
personal data of EU citizens can be stored and processed only within the physical borders of the EU



PI: Personal Information\
PII: Personal Identifiable information\
SPI: Sensitive Personal Information\
PCI: Personal Cardholder Information\
PHI: Protected Health Information

IP: Intellectual property

PIA: Privacy Impact Assessment

* several laws explicitly require PIA (HIPAA. GPDR)

DPIA: Data Protection Impact Assessments&#x20;



1. **Policies** (Overarching Security Policy)
2. **Standards** (specific hardware and software solutions, mechanisms and products)
3. **Procedures** (step-by-step descriptions on how to perform tasks)
4. **Baselines** (minimal implementation methods/levels for security mechanisms and products)
5. **Guidelines** (suggestions)

## Business impact analysis

**Quantitative Analysis**: Use monetary numbers $\
**Qualitative Analysis**: Relative ranking systems (high, low)

* brainstorming
* questionare
* interviews
* delphi technique

Quantitative and Qualitative Analysis can be combined

### Quantitative Risk Analysis

ALE = SLE (AV x EF) x ARO

<table data-header-hidden><thead><tr><th width="108"></th><th></th><th></th><th></th></tr></thead><tbody><tr><td>ALE</td><td>Annualized Loss Expectancy</td><td>annual cost of risk materializing</td><td>$200 * 3 = $600</td></tr><tr><td>SLE</td><td>Single Loss Expectancy</td><td>how much it will cost if risk occurs once</td><td>2000$ * 10% = 200$</td></tr><tr><td>  AV</td><td>Asset Value</td><td>cost of the asset</td><td>CCTV System costs 2`000$ * 10% = 200$</td></tr><tr><td>  EF</td><td>Exposure Factor (0-100%)</td><td>how much the asset loses on value when the incident happens</td><td>three cameras fail during voltage spike:<br>$200 dollar damage -> 10% of AV</td></tr><tr><td>ARO</td><td>Annualized Rate of Occurrence</td><td>How many times each year risk occurs.<br>Every 10 years= 0.1<br>3 times a year = 3</td><td>Happens 3 times per year</td></tr></tbody></table>



Complete control: combination of preventive, detective and corrective controls at a minimum

Continuous Improvement:\
Deming Cycle: Plan, Do, Check, Act

### Risk types

* **Inherent Risk**: level of natural, native or default risk prior to any risk management efforts
* **Residual risk**: Risk after safeguards, security controls and countermeasures are implemented
* **Control Gap:** Difference between the current state of a system’s controls and the desired or necessary state
* **Control Risk/Secondary risk**: risk that is introduced by the introduction of the countermeasure to an environment.
* **Mitigated Risk**: Risk that has been addressed by existing controls
* **Total risk**: without any security controls for all risk
  * Threats \* Vulnerabilities \* Asset value

total risk - control gap = residual risk

ALE1 - ALE2 - ACS = value of safeguard

ALE1: ALE before safeguard\
ALE2: ALE after safeguard\
ACS: Annual Cost of safeguard

## Threat modeling methodologies

STRIDE, PASTA, DREAD

Identify Threats: STRIDE and PASTA

#### **STRIDE**&#x20;

* <mark style="color:$primary;">**threat-focused**</mark> methodology, less strategic.
* designed by Microsoft
* identify and categorize threats / What kind of threats?

| Threat category            | One-line description                                                     | Typical security objective violated |
| -------------------------- | ------------------------------------------------------------------------ | ----------------------------------- |
| **S**poofing               | Pretending to be someone/something you’re not (e.g., forged credentials) | _Authentication_                    |
| **T**ampering              | Unauthorized alteration of data or binaries                              | _Integrity_                         |
| **R**epudiation            | Ability to deny having performed an action (no audit trail)              | _Non-repudiation / Accountability_  |
| **I**nformation Disclosure | Exposing data to parties who shouldn’t see it                            | _Confidentiality_                   |
| **D**enial of Service      | Exhausting resources so legitimate users can’t get service               | _Availability_                      |
| **E**levation of Privilege | Gaining capabilities beyond those intended (e.g., privilege escalation)  | _Authorization_                     |

#### **DREAD**

rate and prioritize the risk of threats

Measuring and ranking the <mark style="color:$primary;">severity of threats</mark>.&#x20;

Often used in combination with STRIDE model. STRIDE identifies the threads, DREAD is used the to rank the severity of threats



| Factor               | Explaination                                                   | Questions                                                                                                                    | Quick scoring hint                                                                       |
| -------------------- | -------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| **D**amage Potential | How bad is it **if the attack succeeds**?                      | ➤ Can the attacker read/modify all data, take full control, or only cause a minor glitch?➤ Is compliance or safety impacted? | Score higher when breach affects life-safety, finances, legal exposure, or entire infra. |
| **R**eproducibility  | How **consistently** can the attack be repeated?               | ➤ Can anyone repeat it at will, or does it require rare timing/luck?➤ Is specialized gear needed each time?                  | “Always works” → high score. “One-in-a-million race condition” → low.                    |
| **E**xploitability   | How **easy** is it to pull off?                                | ➤ Tools, skills, insider access, or physical proximity needed?➤ Public PoC? Automated Metasploit module?                     | Low-skill, remote, automated = high. Needs advanced hardware + insider = low.            |
| **A**ffected Users   | What **percentage of users or systems** could be hit?          | ➤ Is it limited to a niche feature or the entire customer base?➤ Does it span tenants/regions?                               | “Everyone, every time” → high. “Only legacy admin portal users” → lower.                 |
| **D**iscoverability  | How likely is it that **someone will find the vulnerability**? | ➤ Visible in a browser address bar? ➤ Needs source-code access and deep reverse-engineering?➤ Search-engine dork reveals it? | Obvious string in URL = high. Buried in obscure branch logic = low.                      |



#### **PASTA**&#x20;

* focuses on countermeasures based on **asset value**
* **P**rocess for <mark style="color:$primary;">**A**</mark><mark style="color:$primary;">ttack</mark> <mark style="color:$primary;"></mark><mark style="color:$primary;">**S**</mark><mark style="color:$primary;">imulation and</mark> <mark style="color:$primary;"></mark><mark style="color:$primary;">**T**</mark><mark style="color:$primary;">hreat</mark> <mark style="color:$primary;"></mark><mark style="color:$primary;">**A**</mark><mark style="color:$primary;">nalysis</mark>
* Is attacker-focused, **risk-centric** methodology. Strategic perspective.\
  Identify business objectives, technical requirements, compliance issues, business sensitive risks



<table><thead><tr><th width="42">#</th><th>PASTA stage</th><th>One-liner</th></tr></thead><tbody><tr><td>1</td><td><strong>Define Objectives</strong></td><td>Business &#x26; security objectives, compliance drivers</td></tr><tr><td>2</td><td><strong>Define Technical Scope</strong></td><td>Diagram the system, boundaries, tech stack</td></tr><tr><td>3</td><td><strong>Application Decomposition</strong></td><td>Break down components, dataflows, trust boundaries</td></tr><tr><td>4</td><td><strong>Threat Analysis</strong></td><td>Identify realistic attacker goals &#x26; capabilities (STRIDE, CAPEC, etc.)</td></tr><tr><td>5</td><td><strong>Vulnerability &#x26; Weakness Analysis</strong></td><td>Map known vulns (CWE, CVE), abuse cases</td></tr><tr><td>6</td><td><strong>Attack Modelling / Simulation</strong></td><td>Build attack trees, run proofs-of-concept</td></tr><tr><td>7</td><td><strong>Risk &#x26; Impact Analysis</strong></td><td>Quantify likelihood × impact, propose mitigations</td></tr></tbody></table>

### VAST

scalable integration of threat management into an agile programming environment

* **V**isualc
* **A**gile
* **S**imple
* **T**hreat

### Trike

* focused on acceptable risk
* open-source threat modeling process that implements a **requirements model.**

Wassenaar Arrangement: \
Goal: **stop advanced arms or “dual-use” tech** (things with civilian _and_ military value—like AI chips, strong crypto, hacking tools) from ending up in the wrong hands.

The Wassenaar Arrangement is a voluntary pact where the world’s high-tech exporters share and apply a common “do-not-ship-without-a-licence” list so cutting-edge weapons and sensitive tech stay out of dangerous hands.



## Security Control

\
\- **Functional**: must do what it is designed to do\
\- **Assurance**: control can be evaluated to confirm working correctly (testing, monitoring, logging,etc)



## Business continuity management

| Metric                                                        | What it measures                                                                                                                                                                                                 | Typical exam angle                                                |
| ------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------- |
| **Recovery Point Objective (RPO)**                            | _<mark style="color:$primary;">How fresh the data must be</mark> when you come back up._ Expressed as elapsed time between the last good copy of data and the interruption (e.g., “≤ 30 min of data loss”).      | Drives backup/replication frequency.                              |
| **Recovery Time Objective (RTO)**                             | _<mark style="color:$primary;">How quickly the IT service must be restored</mark> to an **operable** state after disruption._ Clock starts at outage, stops when users can log in/use it—even if still degraded. | Determines solution tier (hot site, active-active, etc.).         |
| **Maximum Tolerable Downtime (MTD)**&#x61;ka **MAO/MTO/MTPD** | _Absolute ceiling for being down before the business suffers unacceptable harm._ When you exceed this, you threaten mission, safety, or legal obligations.                                                       | Set during Business Impact Analysis (BIA); RTO must be **≤ MTD**. |



| Metric                         | Meaning                                                                                                | Relationship                                |
| ------------------------------ | ------------------------------------------------------------------------------------------------------ | ------------------------------------------- |
| **Work-Recovery Time (WRT)**   | Time to clear the backlog, validate data, re-configure apps, re-enter transactions, inform users, etc. | **RTO + WRT = MTD** (classic exam formula). |
| **Recovery Time Actual (RTA)** | How long the last real incident or test actually took.                                                 | Used to prove you meet (or miss) the RTO.   |

| Metric                                 | What it tells you                                                                   |
| -------------------------------------- | ----------------------------------------------------------------------------------- |
| **Mean Time to Repair/Restore (MTTR)** | Average time to fix a component or service once it fails.                           |
| **Mean Time Between Failures (MTBF)**  | Average uptime between inherent failures; higher MTBF → greater system reliability. |

## Business continuity planning

(steps not part of CISSP)

1. Strategy **development**
2. Provisions and **processes**
3. Plan **approval**
4. Plan **implentation**
5. Training and **education**

### **Lingo**

BCP bussis continuity planning (BCP)

* how to continue business
* focus on whole business
* cover communications and process more broadly

DRP disaster recovery

* recover from disaster and return to IT infrastructure operation
* focus on technical aspects
* part of BCP

COOP: Continuity of Operations Plan (not part of cissp)

* plan for continuing to do business until the IT infrastructure can be restored



## Categories of Laws

hints: individual vs. state vs. agency



* Criminal Law
  * initiated by police and other law enforcement agencies
  * murder, assault, robbery and arson
  * fines, community service, prison sentences
* Civil Law
  * bulk of the U.S. body of laws
  * contract disputes, real estate transactions, employments matters and estate/probate procedures.
  * usually law enforcement not involved
* Administrative Law
  * deals with the violation of government-imposed regulatory standards (US exchange act, HIPAA, FCC, Export control, etc)
  * regulatory fines
  * published in the Code of Federal Regulations
  * executive orders, <mark style="color:$primary;">policies</mark>, <mark style="color:$primary;">procedures</mark> and <mark style="color:$primary;">regulations</mark> that govern the daily operations of the agency

### Laws

* Comprehensive Crime Control Act \`84
* Computer Fraud and Abuse Act \`86
  * first major piece of cybercrime-specific legilsation
  * protects computer used by to government or in interstate commerce
  * Electronic Communications Privacy Act (ECPA) makes it a crime to invade the electronic privacy of an indiviual
* Computer Security Act `87`
* Economic Espionage Act \`96
  * provides penalties for individuals for the theft of trade secrets
  * harsher penalties apply when stolen info benefits foreign country
* Digital Millennium Copyright Act \`98
  * prohibts the circumvention of copy protection mechanism placed in digital media
  * limits liability of ISP for users
* Government Information Security Reform Act \`00
* Federal Information Security Management Act \`02



GPDR EU / CCPA (California Consumer Privacy Act) US

The **Fourth Amendment** to the U.S. Constitution protects individuals from unreasonable searches and seizures by the government.

GPDR data breaches must be reported within 72 hours. Very high fees if note done. (4% of Umsat)

### Legal, Regulations & Compliance

<table><thead><tr><th width="174">Name of Act / Regulation / Standard</th><th width="128">Acronym</th><th>Year</th><th>Type / Category</th><th>Key CISSP Take-aways (scope, highlights, penalties)</th></tr></thead><tbody><tr><td><strong>Copyright Act (17 U.S.C.)</strong></td><td>–</td><td>1976</td><td>IP law</td><td>Automatic protection on fixation; fair-use; duration life + 70 (works-for-hire = 95/120).</td></tr><tr><td><strong>Digital Millennium Copyright Act</strong></td><td><strong>DMCA</strong></td><td>1998</td><td>IP / cyberlaw</td><td>Anti-circumvention; ISP safe-harbor &#x26; takedown; criminalizes DRM-bypass tools.</td></tr><tr><td>Lanham Act</td><td>–</td><td>1946</td><td>IP law</td><td>Trademark/service-mark registration, dilution, false advertising.</td></tr><tr><td><strong>Economic Espionage Act</strong></td><td><strong>EEA</strong></td><td>1996</td><td>IP / criminal</td><td><p>Felony theft of trade secrets </p><p><br>Adequate steps to ensure trade secrets are well protected must be taken</p></td></tr><tr><td> </td><td> </td><td> </td><td> </td><td> </td></tr><tr><td><strong>Computer Fraud &#x26; Abuse Act</strong></td><td><strong>CFAA</strong></td><td>1986</td><td>Cyber-crime</td><td>“Unauthorized access” to <em>protected computer</em> (federal or interstate); civil &#x26; criminal.</td></tr><tr><td><strong>Computer Security Act</strong></td><td>–</td><td>1987</td><td>Fed. security</td><td>First law requiring agency security plans &#x26; NIST guidance; forerunner of FISMA.</td></tr><tr><td> </td><td> </td><td> </td><td> </td><td> </td></tr><tr><td><strong>Privacy Act</strong></td><td>–</td><td>1974</td><td>Fed. privacy</td><td>Limits U.S. agencies’ PII collection; SORN; access &#x26; amendment rights.</td></tr><tr><td><strong>Electronic Communications Privacy Act</strong></td><td><strong>ECPA</strong></td><td>1986</td><td>Surveillance / privacy</td><td>Wiretap, Stored Comms, Pen-Register titles; governs interception &#x26; disclosure.</td></tr><tr><td><strong>Communications Assistance for Law Enforcement Act</strong></td><td><strong>CALEA</strong></td><td>1994</td><td>Surveillance compliance</td><td>Telcos/VoIP must design for lawful intercept; FCC oversight.</td></tr><tr><td><strong>Children’s Online Privacy Protection Act</strong></td><td><strong>COPPA</strong></td><td>1998</td><td>Privacy for children</td><td>Parental consent &#x26; notice for data on &#x3C; 13; FTC enforcement.</td></tr><tr><td> </td><td> </td><td> </td><td> </td><td> </td></tr><tr><td><strong>Health Insurance Portability &#x26; Accountability Act</strong></td><td><strong>HIPAA</strong></td><td>1996</td><td>Health privacy / security</td><td>Privacy, Security &#x26; Breach Rules; covered entities &#x26; BAs; “minimum necessary.”</td></tr><tr><td><strong>HITECH Act</strong></td><td><strong>HITECH</strong></td><td>2009</td><td>Health privacy</td><td><p>Extends HIPAA;</p><p>pushes to electronic record. mandatory breach notification; </p></td></tr><tr><td><strong>Gramm-Leach-Bliley Act</strong></td><td><strong>GLBA</strong></td><td>1999</td><td><mark style="color:$primary;">Financial</mark> privacy</td><td>Require <mark style="color:$primary;">financial institutions</mark> to protect the <mark style="color:$primary;">security</mark> and confidentiality of<br><mark style="color:$primary;">customer information</mark></td></tr><tr><td><strong>Glass-Steagall Act</strong></td><td>–</td><td>1933</td><td>Banking separation</td><td>Split commercial vs. investment banking; largely repealed by GLBA.</td></tr><tr><td> </td><td> </td><td> </td><td> </td><td> </td></tr><tr><td><strong>USA PATRIOT Act</strong></td><td>–</td><td>2001</td><td>National-security</td><td>Expanded FISA, enhance national security by expanding surveillance powers</td></tr><tr><td><strong>Sarbanes-Oxley Act</strong></td><td><strong>SOX</strong></td><td>2002</td><td>Corp. governance</td><td>Protect <mark style="color:$primary;">investors</mark> by increasing the accuracy of <mark style="color:$primary;">corporate financial reporting</mark></td></tr><tr><td> </td><td> </td><td> </td><td> </td><td> </td></tr><tr><td><strong>Government Information Security Reform Act</strong></td><td><strong>GISRA</strong></td><td>2000</td><td>Gov’t security</td><td>Pilot predecessor to FISMA; NIST control framework.</td></tr><tr><td><strong>Federal Information Security Modernization Act</strong></td><td><strong>FISMA</strong></td><td>2002/14</td><td>Gov’t security</td><td>Requires NIST RMF; ATO; annual OMB reporting.<br>replaced Computer Security Act &#x26; Government Information Security Reform Act</td></tr><tr><td> </td><td> </td><td> </td><td> </td><td> </td></tr><tr><td><strong>General Data Protection Regulation (EU)</strong></td><td><strong>GDPR</strong></td><td>2018</td><td>Global privacy</td><td>Extra-territorial reach; 72-hr breach notice; DPO; fines up to €20 M / 4 % revenue.</td></tr><tr><td><strong>Personal Information Protection and Electronic Documents Act</strong></td><td><strong>PIPEDA</strong></td><td></td><td>Canada</td><td>Regulate how private-sector organizations collet, use, and disclose PI</td></tr><tr><td><strong>Personal Information Protection Law (China)</strong></td><td><strong>PIPL</strong></td><td>2021</td><td>Privacy China</td><td></td></tr><tr><td><strong>Protection of Personal Information Act (Africa)</strong></td><td><strong>POPIA</strong></td><td>2013</td><td>Privacy South Africa</td><td></td></tr><tr><td><strong>Safe Harbor (EU-US, invalidated)</strong></td><td>–</td><td>2000</td><td>Cross-border transfer</td><td>Original EU→US mechanism; struck down by <em>Schrems I</em> (2015).</td></tr><tr><td><strong>Privacy Shield (EU-US, invalidated)</strong></td><td>–</td><td>2016</td><td>Cross-border transfer</td><td>Replacement for Safe Harbor; struck down by <em>Schrems II</em> (2020).<br>Was declared invalid by EU. Orgs must either <mark style="color:$primary;">standard contractual clauses (B2B)</mark> or <mark style="color:$primary;">binding corporate rules (internal company)</mark><br>Both are GDPR templates</td></tr><tr><td><strong>CAN-SPAM Act</strong></td><td>–</td><td>2003</td><td>Consumer protection</td><td>Rules for commercial e-mail (opt-out, header honesty); FTC enforcement.</td></tr><tr><td> </td><td> </td><td> </td><td> </td><td> </td></tr><tr><td><strong>Family Educational Rights &#x26; Privacy Act</strong></td><td><strong>FERPA</strong></td><td>1974</td><td>Education privacy</td><td>Student record rights; limits disclosure without consent.</td></tr><tr><td><strong>Payment Card Industry Data Security Standard</strong></td><td><strong>PCI DSS</strong></td><td>2004</td><td>Industry standard</td><td>12 core requirements; SAQ &#x26; ROC; contractually enforced.</td></tr><tr><td><strong>SOC 2 (AICPA Trust Services Criteria)</strong></td><td><strong>SOC 2</strong></td><td>2010</td><td>Assurance report</td><td>Audit of Security, Availability, PI, Confidentiality, Privacy; Type I vs II.</td></tr><tr><td> </td><td> </td><td> </td><td> </td><td> </td></tr><tr><td><strong>U.S. Patent &#x26; Trademark Office</strong></td><td><strong>USPTO</strong></td><td>–</td><td>Fed. agency</td><td>Registers patents &#x26; trademarks (often a distractor; not a statute).</td></tr><tr><td></td><td></td><td></td><td></td><td></td></tr><tr><td><strong>International Traffic in Arms Regulations</strong></td><td><strong>ITAR</strong></td><td></td><td>strictly <mark style="color:$primary;">military</mark> &#x26; space defense</td><td></td></tr><tr><td><strong>Export Administration Regulations</strong></td><td><strong>EAR</strong></td><td></td><td><mark style="color:$primary;">dual-use</mark> + purely commercial</td><td></td></tr><tr><td><strong>Bank Secrecy Act</strong></td><td><strong>BSA</strong></td><td></td><td>bank, credit unions, casinos</td><td>Combat money laundering and fincancial crimes through record-keeping and reporting requirements</td></tr><tr><td><strong>Freedom of Information Act</strong></td><td><strong>FOIA</strong></td><td></td><td>US federal government agencies</td><td>Provide public access to federal agency records and information</td></tr></tbody></table>





Software license types:

* contractual license
  * written agreements between software and user
* shrink-wrap
  * agreements written of software packaging
  * take effect when user opens package
* click-through agreements
  * require users to accept the terms during installation



## Risk Frameworks

|                                      |                                                      |   |
| ------------------------------------ | ---------------------------------------------------- | - |
| NIST Risk Management Framework (RMF) | Mandatory security requirements for federal agencies |   |
| ISO 31000                            |                                                      |   |
| COSO                                 |                                                      |   |
| ISACA Risk IT                        |                                                      |   |

### **RMF cycles** (NIST 800-37)

* (**Prepare**) to execute the RMF
* **Categorize** information systems
* **Select** security controls
* **Implement** security controls
* **Assess** the security controls
* **Authorize** the system
* **Monitor** security controls

People can see I am always monitoring



AAA:

* Identification
* Authentication
* Authorization
* Accounting



## Supply Chain Risk Management (SCRM)

Process of identifying, accessing and mitigation risks associated with the acquisition of products and services from suppliers and providers

1. integrates with other risk management functions
2. reduces the frequency and severity of supply chain disruptions while improving overall resilience and adaptability
3. ensuring product and service quality, maintaining regulatory compliance



common risks

* product tampering
* counterfeits
* implants

evaluation of 3rd parties

* on-site assessment
* document exchange and review
* process/policy review (request copies)
* third-party audit



mitigations

* third-party assessment and monitoring
* minimum security requirements
  * such as compliance with industry standards, security controls and data protection regulations. Should be in vendor agreement
* service level requirements
  * define and agree upons service level requirements with suppliers and providers: incident response times, sdistarter recovery capabilities, etc
* **Silicon root of trust**: TPM, HSM, secure boot, full disk encryption
* Physically uncloneable function. Hardware-based security primitives that exploit unique character of a device.
* Software bill of materials.



## 2024 Update

**SABSA**: Sherwood Applied Business Security Architecture

* Security Control Framework
  * prescriptive set of cybersecurity safeguards and best practices
  * address the "how"
* Risk Frameworks
  * provide a structured approach to identifying, assessing, prioritizing and managing risk to guide decision-making
  * address the "why"

Sabsa adds another "how" layer by focusing on the structure and design of the security architecture itself



NIST Risk Management Framework (RMF)

* audience: federal governments
* mandatory

NIST Cybersecurity Framework (CSF)

* aimed at private (commercial business)
* optional



**FedRAMP**: <mark style="color:$primary;">Federal</mark> Risk and Authorization Management Program

* <mark style="color:$primary;">government</mark>-wide program
* standardized appoach to security assessment, authorization and continuous monitoring for
* <mark style="color:$primary;">cloud</mark> products and services



### ISO/IEC 27001:2022

* framework for implementing, maintaining, and continually improving an Information Security Mangement System (ISMS)

ISMS is a set of policies, processes and controls that helps orgs protect their information assets

* identifying information assets
  * assign value
  * information risk
* implementing security controls (based on ISO 27002)
* regularly monitoring and measuring effectiveness



### Encryption and Privacy

computer export controls: US companies can't export to Cuba, Iran, North Korea, Sudan and Syria

Encryption Export Controls: Dept of Commerce details limitaions on export of encryption products outside the US

Privacy(US): basis for privacy rights is in the **Fourth Amendment** to the US constitution

Privacy(EU): General Data Protection Regulation (**GDPR**)

* EU customers! also US companies

**PIPL**: Personal Information Protection Law

* China

**POPIA**: Protection of Personal Information Act

* South Africa

Address data collection, security, usage and deletion



Physical unclonable function

* generates a digital fingerprint or signatures based on the unique physical characteristics of an integrated circuit or chip









