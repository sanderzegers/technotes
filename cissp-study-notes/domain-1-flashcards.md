---
description: Material for flashcards
icon: cards
---

# Domain 1: Flashcards

|           |                                                                                                                                                                                  |
| --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ISO 27001 | <p>certifiable, standard that says what an org must have for an <mark style="color:$primary;">ISMS</mark> (policy, risk-based controls, governanace)<br>requirements + audit</p> |
| ISO 27002 | best-practice guide how to implement security controls                                                                                                                           |
| ISO 22301 | certifiable, <mark style="color:$primary;">business continuity management system guidelines</mark>. BIA, strategies, plans, RTO/RPO. -> continuity you can audit                 |
| ISO 28000 | certifiable, <mark style="color:$primary;">supply chain</mark> security management.                                                                                              |
| ISO 31000 | <mark style="color:$primary;">enterprise risk management</mark> guidance. . high-level principles and process for managing risk across the enterprise                            |

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





<table data-header-hidden><thead><tr><th></th><th width="128"></th><th></th><th></th><th></th><th></th><th></th></tr></thead><tbody><tr><td><strong>Type</strong></td><td><strong>What it Protects</strong></td><td><strong>How it's Protected</strong></td><td><strong>Duration</strong></td><td><strong>Disclosure Required</strong></td><td><strong>Loss Risk</strong></td><td><strong>Examples</strong></td></tr><tr><td><strong>Trade Secret</strong></td><td>Confidential info (e.g., formulas, algorithms, methods)</td><td>Kept secret via internal controls, <mark style="color:$primary;">NDAs</mark></td><td><mark style="color:$primary;">Indefinite</mark> (as long as secret is kept)</td><td>No</td><td>If publicly disclosed or reverse-engineered</td><td>Coca-Cola formula, Google algorithm</td></tr><tr><td><strong>Patent</strong></td><td>Inventions, processes, and technical solutions</td><td>Government registration</td><td><mark style="color:$primary;">20 years</mark> (utility patent)</td><td>Yes (full public disclosure)</td><td>Expires after term, can be invalidated</td><td>iPhone hardware design, pharmaceutical drugs</td></tr><tr><td><strong>Copyright</strong></td><td>Original <mark style="color:$primary;">creative works</mark> (code, books, art, music)</td><td>Automatic upon creation (registration optional)</td><td><mark style="color:$primary;">Life of author + 70 years</mark> (U.S.)</td><td>Yes</td><td>Infringement, improper licensing</td><td>Software source code, novels, movies</td></tr><tr><td><strong>Trademark</strong></td><td>Brand names, logos, slogans</td><td><mark style="color:$primary;">Registration or common law use</mark> in commerce</td><td>Indefinite (with use &#x26; renewal every 10 years)</td><td>Yes (public use/association)</td><td>Loss through non-use, dilution, or infringement</td><td>Nike swoosh, Apple logo, McDonald's slogan</td></tr></tbody></table>



1. **Policies** (Overarching Security Policy)
2. **Standards** (specific hardware and software solutions, mechanisms and products)
3. **Procedures** (step-by-step descriptions on how to perform tasks)
4. **Baselines** (minimal implementation methods/levels for security mechanisms and products)
5. **Guidelines** (suggestions)

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



<table><thead><tr><th width="42">#</th><th>PASTA stage</th><th>One-liner</th></tr></thead><tbody><tr><td>1</td><td><strong>Define Business </strong><mark style="color:$primary;"><strong>Objectives</strong></mark></td><td>Business &#x26; security objectives, compliance drivers</td></tr><tr><td>2</td><td><strong>Define </strong><mark style="color:$primary;"><strong>Technical Scope</strong></mark></td><td>Diagram the system, boundaries, tech stack</td></tr><tr><td>3</td><td><mark style="color:$primary;"><strong>Decompose</strong></mark><strong> application</strong></td><td>Break down components, dataflows, trust boundaries</td></tr><tr><td>4</td><td><strong>Analyze </strong><mark style="color:$primary;"><strong>Threats</strong></mark></td><td>Identify realistic attacker goals &#x26; capabilities (STRIDE, CAPEC, etc.)</td></tr><tr><td>5</td><td><strong>Identify </strong><mark style="color:$primary;"><strong>Vulnerabilities</strong></mark></td><td>Map known vulns (CWE, CVE), abuse cases</td></tr><tr><td>6</td><td><strong>Enumerate </strong><mark style="color:$primary;"><strong>attacks</strong></mark></td><td>Build attack trees, run proofs-of-concept</td></tr><tr><td>7</td><td><strong>Perform </strong><mark style="color:$primary;"><strong>risk and impact analysis</strong></mark></td><td>Quantify likelihood × impact, propose mitigations</td></tr></tbody></table>

### VAST

scalable integration of threat management into an agile programming environment

* **V**isual
* **A**gile
* **S**imple **T**reat

Key points:

* Focus: integrating threat modeling into agile and DevOps
* Approach: Visual representations, automation
* Advantages: Well-suited for fast-paced development, easy to use
* Limitations: May not be suitable for complex systems
* Example Use Case: incorporate VAST into their sprint planning.

### TRIKE

Threat and Risk Identification and Knowledge-based Engineering

* focused on acceptable risk and ensuring security requirements are met
* open-source threat modeling process that implements a **requirements model**

### OCTAVE

Operationally Critical Threat, Asset, And Vulnerability Evaluation

* Emphasizes collaboration and a holistic approach to security.
* Leaves security strategy largely to internal IT teams and does not scale well.

## Wassenaar Arrangement:

Goal: **stop advanced arms or “dual-use” tech** (things with civilian _and_ military value—like AI chips, strong crypto, hacking tools) from ending up in the wrong hands.

The Wassenaar Arrangement is a <mark style="color:$primary;">voluntary pact</mark> where the world’s high-tech exporters share and apply a common “do-not-ship-without-a-licence” list so cutting-edge weapons and sensitive tech stay out of dangerous hands.



### Legal, Regulations & Compliance

<table><thead><tr><th width="174">Name of Act / Regulation / Standard</th><th width="128">Acronym</th><th>Type / Category</th><th>Key CISSP Take-aways (scope, highlights, penalties)</th></tr></thead><tbody><tr><td><strong>Copyright Act (17 U.S.C.)</strong></td><td>–</td><td>IP law</td><td>Automatic protection on fixation; fair-use; duration life + 70 (works-for-hire = 95/120).</td></tr><tr><td><strong>Digital Millennium Copyright Act</strong></td><td><strong>DMCA</strong></td><td>IP / cyberlaw</td><td>Anti-circumvention; ISP safe-harbor &#x26; takedown; criminalizes DRM-bypass tools.</td></tr><tr><td><strong>Lanham Act</strong></td><td>–</td><td>IP law</td><td>Trademark/service-mark registration, dilution, false advertising.</td></tr><tr><td><strong>Economic Espionage Act</strong></td><td><strong>EEA</strong></td><td>IP / criminal</td><td><p>Felony theft of trade secrets </p><p><br>Adequate steps to ensure trade secrets are well protected must be taken<br><br>moving from the world of physical to electronic</p></td></tr><tr><td><strong>Identity Theft and Assumption Deterrence Act</strong></td><td></td><td></td><td>makes identity theft a crime. Up to a 15-year prison term and/or a $250k fine)</td></tr><tr><td> </td><td> </td><td> </td><td> </td></tr><tr><td><strong>Computer Fraud &#x26; Abuse Act</strong></td><td><strong>CFAA</strong></td><td>Cyber-crime</td><td>“Unauthorized access” to <em>protected computer</em> (federal or interstate); civil &#x26; criminal.</td></tr><tr><td><strong>Computer Security Act</strong></td><td>–</td><td>Fed. security</td><td>First law requiring agency security plans &#x26; NIST guidance; forerunner of FISMA.</td></tr><tr><td> </td><td> </td><td> </td><td> </td></tr><tr><td><strong>Privacy Act</strong></td><td>–</td><td>Fed. privacy</td><td>Limits U.S. agencies’ PII collection; SORN; access &#x26; amendment rights.<br></td></tr><tr><td><strong>Electronic Communications Privacy Act</strong></td><td><strong>ECPA</strong></td><td>Surveillance / privacy</td><td><mark style="color:$primary;">Wiretap</mark>, Stored Comms, Pen-Register titles; governs interception &#x26; disclosure.</td></tr><tr><td><strong>Communications Assistance for Law Enforcement Act</strong></td><td><strong>CALEA</strong></td><td>Surveillance compliance</td><td>Telcos/VoIP must design for <mark style="color:$primary;">lawful intercept</mark>; FCC oversight.</td></tr><tr><td><strong>Children’s Online Privacy Protection Act</strong></td><td><strong>COPPA</strong></td><td>Privacy for children</td><td>Parental consent &#x26; notice for data on &#x3C; 13; FTC enforcement.</td></tr><tr><td><strong>Family Educational Rights &#x26; Privacy Act</strong></td><td><strong>FERPA</strong></td><td>Education privacy</td><td>Student record rights; limits disclosure without consent.<br>certain privacy rights to students older than 18 and the parents of minor students</td></tr><tr><td> </td><td> </td><td> </td><td> </td></tr><tr><td><strong>Health Insurance Portability &#x26; Accountability Act</strong></td><td><strong>HIPAA</strong></td><td>Health privacy / security</td><td>Privacy, Security &#x26; Breach Rules; covered entities &#x26; BAs; “minimum necessary.”</td></tr><tr><td><strong>HITECH Act</strong></td><td><strong>HITECH</strong></td><td>Health privacy</td><td><p>Extends HIPAA;</p><p>pushes to electronic record. mandatory breach notification; </p></td></tr><tr><td><strong>Gramm-Leach-Bliley Act</strong></td><td><strong>GLBA</strong></td><td><mark style="color:$primary;">Financial</mark> privacy</td><td>Require <mark style="color:$primary;">financial institutions</mark> to protect the <mark style="color:$primary;">security</mark> and <mark style="color:$primary;">confidentiality</mark> of<br><mark style="color:$primary;">customer information</mark></td></tr><tr><td><strong>Glass-Steagall Act</strong></td><td>–</td><td>Banking separation</td><td>Split commercial vs. investment banking; largely repealed by GLBA.</td></tr><tr><td> </td><td> </td><td> </td><td> </td></tr><tr><td><strong>USA PATRIOT Act</strong></td><td>–</td><td>National-security</td><td>Expanded FISA, enhance national security by <mark style="color:$primary;">expanding surveillance</mark> powers</td></tr><tr><td><strong>Sarbanes-Oxley Act</strong></td><td><strong>SOX</strong></td><td>Corp. governance</td><td>Protect <mark style="color:$primary;">investors</mark> by increasing the accuracy of <mark style="color:$primary;">corporate financial reporting</mark></td></tr><tr><td> </td><td> </td><td> </td><td> </td></tr><tr><td><strong>Government Information Security Reform Act</strong></td><td><strong>GISRA</strong></td><td>Gov’t security</td><td>Pilot predecessor to FISMA; NIST control framework.</td></tr><tr><td><strong>Federal Information Security Modernization Act</strong></td><td><strong>FISMA</strong></td><td>Gov’t security</td><td><p>requires gov agencies include that activities of contractors in their security management programs</p><p><br>replaced Computer Security Act &#x26; Government Information Security Reform Act</p></td></tr><tr><td> </td><td> </td><td> </td><td> </td></tr><tr><td><strong>General Data Protection Regulation (EU)</strong></td><td><strong>GDPR</strong></td><td>Global privacy</td><td>Extra-territorial reach; 72-hr breach notice; DPO; fines up to €20 M / 4 % revenue.<br>applies to all organizations world-wide with customers in europe<br>right to be forgotten</td></tr><tr><td><strong>Personal Information Protection and Electronic Documents Act</strong></td><td><strong>PIPEDA</strong></td><td>Canada</td><td>Regulate how private-sector organizations collet, use, and disclose PI</td></tr><tr><td><strong>Personal Information Protection Law (China)</strong></td><td><strong>PIPL</strong></td><td>Privacy China</td><td></td></tr><tr><td><strong>Protection of Personal Information Act (Africa)</strong></td><td><strong>POPIA</strong></td><td>Privacy South Africa</td><td></td></tr><tr><td><strong>Safe Harbor (EU-US, invalidated)</strong></td><td>–</td><td>Cross-border transfer</td><td>Original EU→US mechanism; struck down by <em>Schrems I</em> (2015).</td></tr><tr><td><strong>Privacy Shield (EU-US, invalidated)</strong></td><td>–</td><td>Cross-border transfer</td><td>Replacement for Safe Harbor; struck down by <em>Schrems II</em> (2020).<br>Was declared invalid by EU. Orgs must either <mark style="color:$primary;">standard contractual clauses (B2B)</mark> or <mark style="color:$primary;">binding corporate rules (internal company)</mark><br>Both are GDPR templates</td></tr><tr><td><strong>CAN-SPAM Act</strong></td><td>–</td><td>Consumer protection</td><td>Rules for commercial e-mail (opt-out, header honesty); FTC enforcement.</td></tr><tr><td> </td><td> </td><td> </td><td> </td></tr><tr><td><strong>Payment Card Industry Data Security Standard</strong></td><td><strong>PCI DSS</strong></td><td>Industry standard</td><td>12 core requirements; SAQ &#x26; ROC; contractually enforced.</td></tr><tr><td><strong>SOC 2 (AICPA Trust Services Criteria)</strong></td><td><strong>SOC 2</strong></td><td>Assurance report</td><td>Audit of Security, Availability, PI, Confidentiality, Privacy; Type I vs II.</td></tr><tr><td> </td><td> </td><td> </td><td> </td></tr><tr><td><strong>U.S. Patent &#x26; Trademark Office</strong></td><td><strong>USPTO</strong></td><td>Fed. agency</td><td>Registers patents &#x26; trademarks (often a distractor; not a statute).</td></tr><tr><td></td><td></td><td></td><td></td></tr><tr><td><strong>International Traffic in Arms Regulations</strong></td><td><mark style="color:$primary;"><strong>ITAR</strong></mark></td><td>strictly <mark style="color:$primary;">military</mark> &#x26; space defense</td><td>controls export of items that are specifically designated as military and defense items</td></tr><tr><td><strong>Export Administration Regulations</strong></td><td><mark style="color:$primary;"><strong>EAR</strong></mark></td><td><mark style="color:$primary;">dual-use</mark> + purely <mark style="color:$primary;">commercial</mark></td><td></td></tr><tr><td><strong>Department of Commerce's Bureau of Industry and Security</strong></td><td><strong>BIS</strong></td><td></td><td></td></tr><tr><td><strong>Bank Secrecy Act</strong></td><td><strong>BSA</strong></td><td>bank, credit unions, casinos</td><td>Combat <mark style="color:$primary;">money laundering and financial crimes</mark> through record-keeping and reporting requirements</td></tr><tr><td><strong>Freedom of Information Act</strong></td><td><strong>FOIA</strong></td><td>US federal government agencies</td><td>Provide public access to federal agency records and information</td></tr></tbody></table>



### **RMF cycles** (NIST 800-37)

* (**Prepare**) to execute the RMF
* **Categorize** information systems
* **Select** security controls
* **Implement** security controls
* **Assess** the security controls
* **Authorize** the system
* **Monitor** security controls

People can see I am always monitoring





NIST Risk Management Framework (<mark style="color:$primary;">RMF</mark>)

* audience: federal governments
* mandatory

NIST Cybersecurity Framework (<mark style="color:$primary;">CSF</mark>)

* aimed at private (commercial business)
* optional



**FedRAMP**: <mark style="color:$primary;">Federal</mark> Risk and Authorization Management Program

* <mark style="color:$primary;">government</mark>-wide program
* standardized appoach to security assessment, authorization and continuous monitoring for
* <mark style="color:$primary;">cloud</mark> products and services
