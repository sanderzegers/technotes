---
icon: square-terminal
---

# Domain 8: Software Development Security

Security should be involved at every phase of the development life cycle

SDLC: Software Development Life Cycle\
SLC: System Life cycle

Risk Analysis and threat modeling are very important components of the early phases of SDLC/SLC

Testing should include:

* static
* dynamic
* fuzzing

Certification and accreditation should be performed prior to release/deployment/implementation



|                         |                                   |                                                |
| ----------------------- | --------------------------------- | ---------------------------------------------- |
| Initiation              | Initiation (plan + mgmt approval) |                                                |
|                         | Requirements                      | Risk Analysis                                  |
| Development/Acquisition | Architecture & Design             | Define Security requirements                   |
|                         | Development                       | Build security functionality                   |
| Implementation          | Testing                           | Test Security functionality                    |
|                         | Release / Deployment              | Secure Deployment & Baselines                  |
| Operation / Maintenance | Operation                         | Ongoing Maintenance & Vulnerability Assessment |
| Disposal                | Disposal                          | Secure disposal of Data                        |

SDLC = Initiation + Development + Implementation

SLC = SDLC + Operation + Disposal



|                                    |                                                                                                                                                           |
| ---------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Waterfall                          | <ul><li>complete each  phase of development</li><li>does not allow a previous phase to be revisited</li></ul>                                             |
| Structured Programming Development | <ul><li>said to be foundational to oop</li><li>heavy emphasis on structred control flow</li></ul><p></p>                                                  |
| Agile                              | <ul><li>Divide the development into multiple, rapid iterations of defining, developing, and deploying</li><li>heavy customer interaction</li></ul><p></p> |
| Scaled Agile framework             | <ul><li>adapted version for large organizaations with many teams</li></ul><p></p>                                                                         |
| Spiral Method                      | <ul><li>risk-driven development process that follows an interative model</li><li>includes elements of waterfall</li></ul><p></p>                          |
| Cleanroom                          | <ul><li>focus on defect prevention</li></ul><p></p>                                                                                                       |



Maturity Models

* help improve the development process
* CMMI (Capability Maturity Model Integration)
  * one of most popular



CMMI:

* set of best practices focus on building key capabilities and benchmarking



| Maturity Level            |                                                                                                                                                       | Processes are                |
| ------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------- |
| 0: Incomplete             | phase is unkown and ad hoc, work may not be getting completed                                                                                         | unknown and adhoc            |
| 1: Initial                | reactive and unpredictable stage. Work is getting finished, often coming over budge and late                                                          | reactive and unpredictable   |
| 2: Managed                | projects are managed and planned. Task are performed, key metric are taken                                                                            | managed at the project level |
| 3: Defined                | Org is pro-active. Standard across the org that guide programs, portfolios and projects                                                               | proactive, not reactive      |
| 4: Quantitatively Managed | Controlled and measured stage. Org is driven by data, us it to measure performance improvement objectives. Objectives meet the needs of stake holders | controlled and measured      |
| 5: Optimizing             | stage is flexible and stable. Org is focused on continually improving. Able to pivot when change and opportunity presents themselves                  | stable and flexibel          |



OWASP's Software Assurance Model (SAMM):

three maturity levels:

Level 1: initial implementation

Level 2: Structured Realization

Level 3: Optimized Operation



SAMM looks at software assurance from the high-level perspective of five business functions:

* Governance
* Design
* Implementation
* Verification
* Operations





Operations and Management:

* monitoring, periodic evaluation and patching



Request for change:

* service request
* bug found in system
* new business need



Change Management

* request
* security impact analysis
* approval
* build & test
* notify
* implement
* validate
* version & baseline



Devops

* software development
* operations
* quality assurance

ideally include security as well: DevSecOps



IPT

Integrated Product Team

Fancy word for DevOps.

Goal: create a more agile and repsonsive environment

improve colaboration,&#x20;

Incorporate security into devops:

* plan for security
* strong engagement between developers, operations and security
* engage developers
* develops using secure techniques and frameworks
* automate security test





Canary testing

* push changes to small test group



Smoke Testing

* quick preliminary testing after a change is made to identify any simple failures





CI/CD

Continuous Integration, Delivery and Deployment

* automating, committing code to repo, compile and test automatically
* releases code changes into production without further human intervention



Application Security Testing

SAST: Static application security testing\
DAST: Dynamic application security testing

|      |         |                                                                    |
| ---- | ------- | ------------------------------------------------------------------ |
| SAST | Static  | <ul><li>white box</li><li>examines code</li></ul>                  |
| DAST | Dynamic | <ul><li>Black Box</li><li>Examines app itself</li></ul>            |
|      | Fuzzing | <ul><li>form of dynamic testing</li><li>premise is chaos</li></ul> |

Secure Programming

| Term                  | Mnemonic                                                                                                                                                                  |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Inheritance**       | _Children inherit Dad’s liabilities._                                                                                                                                     |
| **Encapsulation**     | _Put secrets in a capsule._                                                                                                                                               |
| **Polymorphism**      | <p><em>Many forms—one door in.</em><br>A single interface can point to objects of many types; the exact method that runs is chosen at runtime (overriding/overloading</p> |
| **Polyinstantiation** | _One key, many drawers (security labels). (different security levels, see different data)_                                                                                |



Code Obfuscation

Hide or obscure code to protect it from unauthorized viewing

3 types of code obfuscation

* lexical
* data
* control flow



| ... obfuscation |                                                                                                         |                                                                                                      |
| --------------- | ------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------- |
| Lexical         | modifies the look of the code                                                                           | <ul><li>change comments</li><li>removeing debugging info</li><li>change format of the code</li></ul> |
| data            | modifies the data structure                                                                             |                                                                                                      |
| control flow    | modify flow of control (reordering statements, methods, loops, create irrelevant conditional statements |                                                                                                      |





Database



ACID: atomicity, consistency, isolation, durability

|   |             |                                                          |
| - | ----------- | -------------------------------------------------------- |
| A | Atomicity   | All changes take effect or none at all!                  |
| C | Consistency | Constisten with the Rules                                |
| I | Isolation   | Transactions are invisible to other users until complete |
| D | Durability  | Completed changes will not be lost                       |

column/fields = attributes\
rows/records = tuples



CI/CD

SOAR&#x20;

SCM



Software escrow: A three-party legal arrangement in which the vendor/developer deposits source code — plus build scripts, libraries, keys, and documentation — with a neutral escrow agent. If pre-defined “release conditions” occur, the agent hands the materials to the customer/licensee.



Acquiring Software:

* should be taken as seriously as developing software
* phases
  * planning/requirements
  * contracting
  * accxeptance
  * monitoring
  * follow-on



COTS: Commercial-Off-the-Shelf

* functionality of product can be more easily verified/confirmed
* comparison of products can be mdate
* existing customers can be contacted
* software updates and patches are likely more readily available

Cons:

* no white box testing
* vendor could go out of business
* support and related issues
* missing features and functionality
* vulnerbailities due to larger user base





Secure code guidelines

|                        |                                                                                |                |
| ---------------------- | ------------------------------------------------------------------------------ | -------------- |
| Covert Channels        | Unintentional communications path. Two types: timing and storage               |                |
| Buffer Overflows       |                                                                                |                |
| Memory / Object Reuse  |                                                                                |                |
| Executable Mobile Code | code that is downloaded to system and then run                                 |                |
| TOCTOU                 | time-of-check time-of-use                                                      | race condition |
| Backdoors/Trapdoors    | maintenance hooks forgotten to be removed                                      |                |
| Malformed iNput        | Web Apps                                                                       |                |
| Citizen Developers     | normal users have access to powerful tools with security skills. (SQL Queries) |                |

