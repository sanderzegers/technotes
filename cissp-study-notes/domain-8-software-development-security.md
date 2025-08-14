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

## Systems development modules

|                                    |                                                                                                                                                                                                                                           |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Waterfall                          | <ul><li>complete each  phase of development</li><li>7 stages</li><li>does not allow a previous phase to be revisited</li><li>sequential</li></ul>                                                                                         |
| Structured Programming Development | <ul><li>said to be foundational to OOP</li><li>heavy emphasis on structured control flow</li></ul>                                                                                                                                        |
| Agile                              | <ul><li>Divide the development into multiple, rapid <mark style="color:$primary;">iterations</mark> of defining, developing, and deploying</li><li>heavy customer interaction</li></ul>                                                   |
| Scaled Agile framework             | <ul><li>adapted version for large organizations with many teams</li></ul>                                                                                                                                                                 |
| Spiral Method                      | <ul><li>risk-driven development process that follows an <mark style="color:$primary;">iterative</mark> model</li><li>includes elements of waterfall</li><li>allows developers to returning to planing stages as demands changes</li></ul> |
| Cleanroom                          | <ul><li>focus on defect prevention</li></ul>                                                                                                                                                                                              |

### Waterfall

* system requirements
* software requirements
* preliminary design
* detailed design
* code and debug
* testing
* ops & maintenance



## Maturity Models

* help improve the development process
* CMMI (Capability Maturity Model Integration)
  * one of most popular



CMMI:

* set of best practices focus on building key capabilities and benchmarking

5-step model for measuring software development in organizations:

<table><thead><tr><th width="176">Maturity Level</th><th></th><th></th><th>Processes are</th></tr></thead><tbody><tr><td>0: Incomplete</td><td></td><td>phase is unkown and ad hoc, work may not be getting completed</td><td>unknown and adhoc</td></tr><tr><td>1: Initial</td><td>No plan</td><td>reactive and unpredictable stage. Work is getting finished, often coming over budge and late</td><td>reactive and unpredictable</td></tr><tr><td>2: Repeatable</td><td>Basic lifecycle management</td><td>projects are managed and planned. Task are performed, key metric are taken</td><td>managed at the project level</td></tr><tr><td>3: Defined</td><td>formal, documented SW development process</td><td>Org is pro-active. Standard across the org that guide programs, portfolios and projects</td><td>proactive, not reactive</td></tr><tr><td>4: Managed</td><td>Quantitatively Managed</td><td>Controlled and measured stage. Org is driven by data, us it to measure performance improvement objectives. Objectives meet the needs of stake holders</td><td>controlled and measured</td></tr><tr><td>5: Optimizing</td><td>continous development process, w/ feedback loops . CI/CD</td><td>stage is flexible and stable. Org is focused on continually improving. Able to pivot when change and opportunity presents themselves</td><td>stable and flexibel</td></tr></tbody></table>

## IDEAL model

guides organizations through continuous improvement—perfect for rolling out or maturing a security program

|                  |                                                                                                                                  |   |
| ---------------- | -------------------------------------------------------------------------------------------------------------------------------- | - |
| **I**nitiating   | <mark style="color:$primary;">business reasons outlined,</mark> support & infrastructure for initiate put place                  |   |
| **D**iagnosing   | Engineers <mark style="color:$primary;">analyze current state</mark> of org & make recommendations for change                    |   |
| **E**stablishing | Org takes recommendations & <mark style="color:$primary;">develops plan</mark> to achieve those changes                          |   |
| **A**cting       | <mark style="color:$primary;">Plan put into action</mark>. Org develops solutions, tests, refines & implements                   |   |
| **L**earning     | Org <mark style="color:$primary;">continuously analyzes efforts and results</mark>, proposes new actions to drive better results |   |

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



## Change Management

* request
* security impact analysis
* approval
* build & test
* notify
* implement
* validate
* version & baseline



Request control

Change control

Release control

## Devops

* software development
* operations
* quality assurance

ideally include security as well: DevSecOps



## IPT

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



**Canary testing**

* push changes to small test group

**Smoke Testing**

* quick preliminary testing after a change is made to identify any simple failures

## CI/CD

Continuous Integration, Delivery and Deployment (or delivery)

* automating, committing code to repo, compile and test automatically
* releases code changes into production without further human intervention

automate vulnerability scanning in your ci/cd pipeline

## Application Security Testing

SAST: Static application security testing\
DAST: Dynamic application security testing

Best to combine both

|      |         |                                                                    |
| ---- | ------- | ------------------------------------------------------------------ |
| SAST | Static  | <ul><li>white box</li><li>examines code</li></ul>                  |
| DAST | Dynamic | <ul><li>Black Box</li><li>Examines app itself</li></ul>            |
|      | Fuzzing | <ul><li>form of dynamic testing</li><li>premise is chaos</li></ul> |

## Secure Programming

| Term                  | Mnemonic                                                                                                                                                                  |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Inheritance**       | _Children inherit Dad’s liabilities._                                                                                                                                     |
| **Encapsulation**     | _Put secrets in a capsule._                                                                                                                                               |
| **Polymorphism**      | <p><em>Many forms—one door in.</em><br>A single interface can point to objects of many types; the exact method that runs is chosen at runtime (overriding/overloading</p> |
| **Polyinstantiation** | _One key, many drawers (security labels). (different security levels, see different data)_                                                                                |



## Code Obfuscation

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





## Database

ACID: atomicity, consistency, isolation, durability

|   |             |                                                          |
| - | ----------- | -------------------------------------------------------- |
| A | Atomicity   | All changes take effect or none at all!                  |
| C | Consistency | Consistent with the Rules                                |
| I | Isolation   | Transactions are invisible to other users until complete |
| D | Durability  | Completed changes will not be lost                       |

tables = relations\
column/fields = attributes\
rows/records = tuples



CI/CD

SOAR&#x20;

SCM



Software escrow: A three-party legal arrangement in which the vendor/developer deposits source code — plus build scripts, libraries, keys, and documentation — with a neutral escrow agent. If pre-defined “release conditions” occur, the agent hands the materials to the customer/licensee.

## RDBMS threats

* aggregation attack
  * create sensitive information by combining non-sensitive data from separate sources
    * single transfer / entry of soldiers in military base
* inference
  * deduce or assume senstive information from observing non-sensitive pieces of information
    * total salary and information when employees enter company



## Acquiring Software

* should be taken as seriously as developing software
* phases
  * planning/requirements
  * contracting
  * acceptance
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



## Secure code guidelines

|                        |                                                                                |                |
| ---------------------- | ------------------------------------------------------------------------------ | -------------- |
| Covert Channels        | Unintentional communications path. Two types: timing and storage               |                |
| Buffer Overflows       |                                                                                |                |
| Memory / Object Reuse  |                                                                                |                |
| Executable Mobile Code | code that is downloaded to system and then run                                 |                |
| TOCTOU                 | time-of-check time-of-use                                                      | race condition |
| Backdoors/Trapdoors    | maintenance hooks forgotten to be removed                                      |                |
| Malformed input        | Web Apps                                                                       |                |
| Citizen Developers     | normal users have access to powerful tools with security skills. (SQL Queries) |                |

## ML and neural networks

**Expert systems**

* consist of two main components
* a knowledge base. contains a series of if/then rules
* a inference engines. uses that information to draw conslusions about other data

**Machine learning**

* algorithmically discover knowledge from datasets

**Neural Networks**

* simulate function of the human mind
* require extensive training



## Concentric Circle Security

* **Layered & independent rings**: multiple controls around the asset so no single failure exposes it&#x20;
* true defense-in-depth

