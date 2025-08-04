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
* non-repudiation
  * Someone cannot deny their actions (sending messages, making transanctions)

opposite of / failure of CIA = DAD:

* Disclosure
* Alternation
* Destruction



Security Roles

|                       |            |                                                  |
| --------------------- | ---------- | ------------------------------------------------ |
| Security Architect    | Design     | responsible for enterprise security architecture |
| Security Practitioner | Operation  | responsible for tactical and operation           |
| Security Professional | Management | managerial oversight                             |
|                       |            |                                                  |



**Governance**: act of governing or overseeing the process of directing something. It's about improving the company with processes, new services, improving margins, meeting compliance requirements etc.

Policies = "corporate law"

accountability vs responsibility



Security Frameworks: NIST, ISO, COBIT, ITIL

COBIT: Control Objectives for Information and Related Technology.&#x20;

* documented set of best IT security practices crafted by <mark style="color:$primary;">ISACA</mark>
* prescribes goals and requirements for security controls and encourage <mark style="color:$primary;">mapping of it security ideals to business objects.</mark>



NIST 800-53: Security and Privacy Controls for Information Systems and Organizations

CIS: cisecurity. provides OS, apps, hardware security configurations

NIST Risk Management Framework:

* mandatory requirements for federal agencies.

NIST Cybersecurity Framework (CSF)





Due care vs Due diligence

* Due care is the <mark style="color:$primary;">responsible protection of assets</mark>
* Due diligence is the ability to <mark style="color:$primary;">prove due care</mark>
  * <mark style="color:$primary;">establish a plan, policy and process to protect interests of an organization</mark>



📊 Intellectual Property Comparison Table

<table data-header-hidden><thead><tr><th></th><th width="128"></th><th></th><th></th><th></th><th></th><th></th></tr></thead><tbody><tr><td><strong>Type</strong></td><td><strong>What it Protects</strong></td><td><strong>How it's Protected</strong></td><td><strong>Duration</strong></td><td><strong>Disclosure Required</strong></td><td><strong>Loss Risk</strong></td><td><strong>Examples</strong></td></tr><tr><td><strong>Trade Secret</strong></td><td>Confidential info (e.g., formulas, algorithms, methods)</td><td>Kept secret via internal controls, <mark style="color:$primary;">NDAs</mark></td><td><mark style="color:$primary;">Indefinite</mark> (as long as secret is kept)</td><td>No</td><td>If publicly disclosed or reverse-engineered</td><td>Coca-Cola formula, Google algorithm</td></tr><tr><td><strong>Patent</strong></td><td>Inventions, processes, and technical solutions</td><td>Government registration</td><td><mark style="color:$primary;">20 years</mark> (utility patent)</td><td>Yes (full public disclosure)</td><td>Expires after term, can be invalidated</td><td>iPhone hardware design, pharmaceutical drugs</td></tr><tr><td><strong>Copyright</strong></td><td>Original <mark style="color:$primary;">creative works</mark> (code, books, art, music)</td><td>Automatic upon creation (registration optional)</td><td><mark style="color:$primary;">Life of author + 70 years</mark> (U.S.)</td><td>Yes</td><td>Infringement, improper licensing</td><td>Software source code, novels, movies</td></tr><tr><td><strong>Trademark</strong></td><td>Brand names, logos, slogans</td><td><mark style="color:$primary;">Registration or common law use</mark> in commerce</td><td>Indefinite (with use &#x26; renewal)</td><td>Yes (public use/association)</td><td>Loss through non-use, dilution, or infringement</td><td>Nike swoosh, Apple logo, McDonald's slogan</td></tr></tbody></table>



Data Residency regulation: GDPR (General Data Protection Regulation)\
personal data of EU citizens can be stored and processed only within the physical borders of the EU



PI: Personal Information\
PII: Personal Identifiable information\
SPI: Sensitive Personal Information\
PCI: Personal Cardholder Information\
PHI: Protected Health Information

IP: Intellectual property

PIA: Privacy Impact Assessment\
DPIA: Data Protection Impact Assessments&#x20;



Policies (Overarching Security Policy)\
^- Standards (specific hardware and software solutions, mechanisms and products)\
^- Procedures (step-by-step descriptions on how to perform tasks)\
^- Baselines (minimal implementation methods/levels for security mechanisms and products)\
^- Guidelines (suggestions)

**Quantitative Analysis**: Use monetary numbers $\
**Qualitative Analysis**: Relative ranking systems (high, low)



ALE: Annualized Loss Expectancy\
SLE: Single Loss Expectancy\
\- AV: Asset Value\
\- EF: Exposure Factor (0-100%)\
\
ARO: Annualized Rate of Occurrence

ALE = SLE (AV x EF) x ARO

Complete control: combination of preventive, detective and corrective controls at a minimum

Continuous Improvement:\
Deming Cycle: Plan, Do, Check, Act

Risk types:

* Inherent Risk: level of natural, native or default risk prior to any risk management efforts
* Residual risk: Risk after safeguards, security controls and countermeasures are implemented
* Control Risk: risk that is introduced by the introduction of the countermeasure to an environment.
* Mitigated Risk: Risk that has been addressed by existing controls

### Threat modeling methodologies

STRIDE, PASTA, DREAD

Identify Threats: STRIDE and PASTA

\
**STRIDE**&#x20;

* <mark style="color:$primary;">**threat-focused**</mark> methodology, less strategic.
* designed by Microsoft
* type of threats

| Threat category            | One-line description                                                     | Typical security objective violated |
| -------------------------- | ------------------------------------------------------------------------ | ----------------------------------- |
| **S**poofing               | Pretending to be someone/something you’re not (e.g., forged credentials) | _Authentication_                    |
| **T**ampering              | Unauthorized alteration of data or binaries                              | _Integrity_                         |
| **R**epudiation            | Ability to deny having performed an action (no audit trail)              | _Non-repudiation / Accountability_  |
| **I**nformation Disclosure | Exposing data to parties who shouldn’t see it                            | _Confidentiality_                   |
| **D**enial of Service      | Exhausting resources so legitimate users can’t get service               | _Availability_                      |
| **E**levation of Privilege | Gaining capabilities beyond those intended (e.g., privilege escalation)  | _Authorization_                     |

\
**DREAD**

* severity scoring of threats

Measuring and ranking the <mark style="color:$primary;">severity of threats</mark>.&#x20;

Often used in combination with STRIDE model. STRIDE identifies the threads, DREAD is used the to rank the severity of threats

| Factor               | Explaination                                                   | Questions                                                                                                                    | Quick scoring hint                                                                       |
| -------------------- | -------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| **D**amage Potential | How bad is it **if the attack succeeds**?                      | ➤ Can the attacker read/modify all data, take full control, or only cause a minor glitch?➤ Is compliance or safety impacted? | Score higher when breach affects life-safety, finances, legal exposure, or entire infra. |
| **R**eproducibility  | How **consistently** can the attack be repeated?               | ➤ Can anyone repeat it at will, or does it require rare timing/luck?➤ Is specialized gear needed each time?                  | “Always works” → high score. “One-in-a-million race condition” → low.                    |
| **E**xploitability   | How **easy** is it to pull off?                                | ➤ Tools, skills, insider access, or physical proximity needed?➤ Public PoC? Automated Metasploit module?                     | Low-skill, remote, automated = high. Needs advanced hardware + insider = low.            |
| **A**ffected Users   | What **percentage of users or systems** could be hit?          | ➤ Is it limited to a niche feature or the entire customer base?➤ Does it span tenants/regions?                               | “Everyone, every time” → high. “Only legacy admin portal users” → lower.                 |
| **D**iscoverability  | How likely is it that **someone will find the vulnerability**? | ➤ Visible in a browser address bar? ➤ Needs source-code access and deep reverse-engineering?➤ Search-engine dork reveals it? | Obvious string in URL = high. Buried in obscure branch logic = low.                      |



\
**PASTA**&#x20;

* Process for Attack Simulation and Threat Analysis
* Is attacker-focused, **risk-centric** methodology. Strategic perspective.\
  Identify business objectives, technical requirements, compliance issues, business sensitive risks

<table><thead><tr><th width="64">#</th><th>PASTA stage</th><th>One-liner</th></tr></thead><tbody><tr><td>1</td><td><strong>Define Objectives</strong></td><td>Business &#x26; security objectives, compliance drivers</td></tr><tr><td>2</td><td><strong>Define Technical Scope</strong></td><td>Diagram the system, boundaries, tech stack</td></tr><tr><td>3</td><td><strong>Application Decomposition</strong></td><td>Break down components, dataflows, trust boundaries</td></tr><tr><td>4</td><td><strong>Threat Analysis</strong></td><td>Identify realistic attacker goals &#x26; capabilities (STRIDE, CAPEC, etc.)</td></tr><tr><td>5</td><td><strong>Vulnerability &#x26; Weakness Analysis</strong></td><td>Map known vulns (CWE, CVE), abuse cases</td></tr><tr><td>6</td><td><strong>Attack Modelling / Simulation</strong></td><td>Build attack trees, run proofs-of-concept</td></tr><tr><td>7</td><td><strong>Risk &#x26; Impact Analysis</strong></td><td>Quantify likelihood × impact, propose mitigations</td></tr></tbody></table>



Wassenaar Arrangement: \
Goal: **stop advanced arms or “dual-use” tech** (things with civilian _and_ military value—like AI chips, strong crypto, hacking tools) from ending up in the wrong hands.

The Wassenaar Arrangement is a voluntary pact where the world’s high-tech exporters share and apply a common “do-not-ship-without-a-licence” list so cutting-edge weapons and sensitive tech stay out of dangerous hands.



Security Control:\
\- Functional: must do what it is designed to do\
\- Assurance: control can be evaluated to confirm working correctly (testing, monitoring, logging,etc)

### Business continuity management

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



### Categories of Laws

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
  * executive orders, policies, procedures and regulations that govern the daily operations of the agency

Laws

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

The Fourth Amendment to the U.S. Constitution protects individuals from unreasonable searches and seizures by the government.



Other

|                                                     |                                                                    |   |
| --------------------------------------------------- | ------------------------------------------------------------------ | - |
| Copyright Act                                       |                                                                    |   |
| Lanham Act                                          |                                                                    |   |
| Glass-Steagall Act                                  |                                                                    |   |
| Economic Espionage Act                              |                                                                    |   |
| Privacy Act                                         |                                                                    |   |
| HITECH Act                                          | Health Information Technology for Economic and Clinical Health Act |   |
|                                                     | CALEA                                                              |   |
| Children’s Online Privacy Protection Act            | COPPA                                                              |   |
| Electronic Communications Privacy Act               | ECPA                                                               |   |
|                                                     | USPTO                                                              |   |
| Digital Millennium Copyright Act                    | DMCA                                                               |   |
| Gramm Leach Bliley Act                              | GLBA                                                               |   |
| USA Patriot ACT                                     |                                                                    |   |
| Privacy Shield                                      |                                                                    |   |
| Safe Harbor                                         |                                                                    |   |
|                                                     | SOX                                                                |   |
| Health Insurance Portability and Accountability Act | HIPAA                                                              |   |
| Family Educational Rights and Privacy Act           | FERPA                                                              |   |
|                                                     | FISMA                                                              |   |
|                                                     | PCI DSS                                                            |   |
|                                                     | GISRA                                                              |   |
|                                                     | SOC2                                                               |   |







Software license types:

* contractual license
  * written agreements between software and user
* shrink-wrap
  * agreements written of software packaging
  * take effect when user opens package
* click-through agreements
  * require users to accept the terms during installation

