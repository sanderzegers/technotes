---
icon: fingerprint
---

# Domain 5: Identity & Access Management

Access control is the collection of mechanism that work together to protect the assets of an organization and, at the same time, allow controlled access to authorized subjects

* specify which **users** can access the system
* what **resources** they can access
* what **operations** they can perform
* **accountability**



Fundamental access control principles

* Need to know
* Least privilege
* Separation of Duties (prevent error and fraud)



Access control concerns ALL assets:\
Facilities, Systems/Devices, Information, Personnel, Applications



Reference Monitor concept:\
\- Subject\
\- Mediation (Rules and logs & monitor)\
\- Object

\
Implementation of the RMC = Security Kernel

Logical Access Modes: Action permissions that can be applied to an object:

* create
* update
* read
* read/write
* execute
* delete



Groups vs Roles: two different approaches

Role: set of permissions that is usally associated with a specific job.\
focused around the function of the job.

Groups: much more granular

Access control administrator: Centralized vs Decentralized (vs hybrid)

Centralized: one central system controls access. One username and password. single point of failure, and potential target

Hybrid: Used most. due to legacy system which cannot be integrated

Decentralized: seperate username and password on various systems. lack of standardization. Overlapping right and security holes.



Kim Cameron’s **Seven Laws of Identity** (2005)

| #     | Law                                          | One-line meaning (CISSP takeaway)                                                                                                                                                                                                                                                                                           |
| ----- | -------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **1** | **User Control & Consent**                   | The user—not the system—decides when and what identity data are released.                                                                                                                                                                                                                                                   |
| **2** | **Minimal Disclosure for a Constrained Use** | Share the least identifying information, only for the specific purpose at hand (d**ata-minimization**).                                                                                                                                                                                                                     |
| **3** | **Justifiable Parties**                      | Reveal data only to parties that have a legitimate, necessary role in the transaction.                                                                                                                                                                                                                                      |
| **4** | **Directed Identity**                        | <p>Support <strong>both</strong> public, reusable identifiers <em>and</em> private, one-off identifiers to avoid unwanted correlation.<br>Example: apple id (with real e-mail address or unique privaterelay address per service)<br>Goal: give people control over how linkable their identifiers are across contexts.</p> |
| **5** | **Pluralism of Operators & Technologies**    | Design for an ecosystem of many identity providers and technical approaches that can interoperate.                                                                                                                                                                                                                          |
| **6** | **Human Integration**                        | Make the human user an explicit, intuitive part of the trust loop (clear prompts, recognisable UX cues). MS authenticator number matching.                                                                                                                                                                                  |
| **7** | **Consistent Experience Across Contexts**    | Present a uniform, predictable identity experience no matter the site, device, or app.                                                                                                                                                                                                                                      |



Access control service: identification, authentication, authorization, accountability (IAAA)

User identification needs to be:

* unique
* nondescriptive of rule (admin)
* issued and used securely (password manager)

3FA: you know, you have, you are

OTP: Asynchronous vs Synchronous

Asynchronous not very common. Server sends challenge. (synchronization makes this complex)

Synchronous: Without challenge user sends tokens (TOTP)



CER (Crossover Error Rate) measures accuracy of biometric system. Intersection between type 1 (false reject) and type 2 (false acceptance)

FFR (false rejection rate)\
FAR (false acceptance rate) in percentage

good biometric systems will use one-wy mathematical functions to create a represention: template.\
biometric data should never be stored.

1:N for identification. Looks up fingerprint in DB\
1:1 for authentication. Compares fingerprint with already partial logged in user

Iris scanner scans colored ring around an eye\
Retina scanner scans vein pattern at the back of the eye. Most accurate biometric auth system. scanning is rather unpleasent. Rubber eye cup & Flashlight.  Can reveal medical issues.

Behavioral: Voice, Signature (writing), Keystroke, Gait (how person walks)



SESAME is Kerberos successor&#x20;

Kerberos (Different A's!!!):

* Authentication
* Accounting
* Auditing

AS (Authentication Service)

1. TGT (from AS)
2. TGS (Ticket Granting Service)

KDC (Key Distribution Center) = TGT + TGS

TOCTOU: Time of Checkout, Time of Use attack

SESAME: Secure European System for Applications in a Multi-Vendor Environment: Support asymmetric encryption in addition to symmetric. Issues multiple ticket which mitagates TOCTOU

AAL: Authenticator Assurance Level

AAL: refers to the strength of authentication processes and systems\
AAL1: Least roboust, AAL3 most robust

|      |                                                                                                                                                |
| ---- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| AAL1 | <ul><li>some assurance</li><li>single-factor auth</li><li>secure auth protocol</li></ul>                                                       |
| AAL2 | <ul><li>high confidence</li><li>mfa</li><li>approved crypto techniquea</li></ul>                                                               |
| AAL3 | <ul><li>very high confidence</li><li>hardware cyrpto authentictaor providing proof of possession of key and impersonation resistence</li></ul> |



FIM: Federated Identity Management

Active Directory is a FIM

Builds on trusted relationship between different entities

Major Protocols:

* SAML (both)
* WS-Federation (both)
* OpenID (authentication)
* Oauth (authorization)



WS-Federation: created by IBM, Microsoft, and Verisign.\
Defined as standard by OASIS

OpenID does the authentication and Oauth the authorization. Often both protocols work together.

SAML uses assertion tickets or tokens.\
assertions are written in XML



**Principle of Access Control** refers to accountability:

* User must be **uniquely identified**
* Users must be properly **authenticated**
* Users must be properly **authorized**
* All actions should be **logged and monitored**



IDaas: identity as a service

Common capabilities:

* Provisioning
* Administration
* Single-Sign on&#x20;
* MFA
* Directory Services
* On premise and in the cloud



Identity types

|                    | Account stored in                      | Authenticated against |
| ------------------ | -------------------------------------- | --------------------- |
| Cloud Identity     | Cloud                                  | Cloud Service         |
| Synced Identity    | Cloud synced to local or vice-versa    | Either Cloud or Local |
| Linked Identity    | Two separate accounts which are linked | Either Cloud or Local |
| Federated Identity | IDP                                    | IDP                   |

IdAAS Risks:

* Availability of service
* Protection of critical identity data (PII)
* Entrusting third party with senstive or proprietary data



DAC: Discretionary Access Control: asset owner determines who can access the asset: access is given at the **discretion** of the owner

Three different philosophies and methodologies:

* **Discretionary**: Owner decides
* **Mandatory**: System decides
* **Non-Discretionary**: Someone other than the owners decides



Access Control Summary:

|                                                          |                                                                                     |   |
| -------------------------------------------------------- | ----------------------------------------------------------------------------------- | - |
| Discretionary Access Control (DAC)                       | Owner determines access rules                                                       |   |
| <ul><li>Role-Based Access Control (RABC)</li></ul>       | Access to resources is based on user roles (firewall admin, accounts payable clerk) |   |
| <ul><li>Rule-Based Access Control (ABAC)</li></ul>       | Access to resources is based on a set of rules (ACL)                                |   |
| <ul><li>Attribute-Based Access Controls (ABAC)</li></ul> | Access is based on attributes (OS browser, IP address)                              |   |
| Mandatory Access Control (MAC)                           | Systems determines access rules based on **labels.**                                |   |
| Risk-Based Access Control                                | Risk profil based on (ip address, time of access, type of access request)           |   |



Rule-Based Access Control

A single rule for every user and single asset.\
Very granular control but very high admin effort



Role-Based Acccess

Assign users to roles/groups

| Level            | One-liner description                                            | Where you still see it                |
| ---------------- | ---------------------------------------------------------------- | ------------------------------------- |
| **Non-RBAC**     | Direct, per-user permissions; no roles at all.                   | Small or legacy apps.                 |
| **Limited RBAC** | Each application has its **own** roles, isolated from others.    | Separate, silo-ed systems.            |
| **Hybrid RBAC**  | Some shared roles span a few apps, but not the whole enterprise. | Orgs part-way through an IAM rollout. |
| **Full RBAC**    | Enterprise-wide roles drive access to every integrated system.   | Mature, regulated environments.       |

Attribute/Context-Based Access Control: Risk profil based on (ip address, time of access, type of access request)

XACML: eXtensible Access Control Markup Language\
Standard defines an attribute-based access control policy language



MAC: Mandatory Access Control

Very rarely used. Primarily Government. Where confidentiality is key

Every asset must be classified, every user should be assigned a clearance level

Classification lables: public, secret top secret...



Non-Discretionary Access Control:

somebody other than asset owner determines who gets access\
Should be avoided whenever possible

example: IT admin assigns permissions



Access policy enforcement:

* PEP: policy enforcement point
  * Gatekeep/application that enforces and requests permissions (from PDP)
* PDP: policy decision point
  * makes decisions on the authorization request sent by PEP



Vendor provisioning might include a security review component.



Identity life cycle:

Provision

* backgorund check, conirming skill, id proof

Review

* periodic check appropriate access
* annually minimum
* reviewed by owner
* admin review more frequently
* risk should drive frequency

Revocation



Service Account Management

* limit to signle purpose, reduce privileges
