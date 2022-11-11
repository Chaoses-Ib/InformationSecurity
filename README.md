# Information Security
**Information security (InfoSec)** is the practice of protecting information by mitigating information risks. It is part of information risk management. It typically involves preventing or reducing the probability of unauthorized/inappropriate access to data, or the unlawful use, disclosure, disruption, deletion, corruption, modification, inspection, recording, or devaluation of information.

Information security's primary focus is the balanced protection of the confidentiality, integrity, and availability (CIA) of data while maintaining a focus on efficient policy implementation, all without hampering organization productivity.[^wiki]

Objects:
- CIA
  - Confidentiality (保密性)  
    Confidentiality is the property, that information is not made available or disclosed to unauthorized individuals, entities, or processes.[^wiki]
  - Integrity (完整性)  
    Integrity means maintaining and assuring the accuracy and completeness of data over its entire lifecycle.[^wiki] The information cannot be modified in an unauthorized or undetected manner.
  - Availability (可用性)  
    For any information system to serve its purpose, the information must be available when it is needed.[^wiki]
- Non-repudiation (不可否认性)  
  The information's author cannot successfully dispute its authorship or the validity of an associated contract.
- Authenticity (真实性)  
  The information's recipient should be able to identify the sender and verify that the purported sender actually sent the message.
- Privacy (隐私)  
  Confidentiality is a component of privacy that implements to protect our data from unauthorized viewers.[^wiki]
- Accountability (问责性)
- Reliability (可靠性)  
  Availability measures the ability of a piece of equipment to be operated if needed, while reliability measures the ability of a piece of equipment to perform its intended function for a specific interval without failure.[^upkeep]
- Auditability (可审计性)

Standards:
- [Trusted Computer System Evaluation Criteria (TCSEC)](Standards/TCSEC.md)
- [Common Criteria (CC)](Standards/Common%20Criteria.md)

## [Cryptology](https://github.com/Chaoses-Ib/Cryptology)

## [Steganography](Steganography/README.md)
- [Least Significant Bit Encoding](Steganography/Least%20Significant%20Bit%20Encoding.md)

## Access control
**Access control (AC, 访问控制)** is the selective restriction of access to a place or other resource, while access management describes the process. Permission to access a resource is called **authorization**.[^wiki]

Three steps[^wiki]:
1. Identification (识别)  
   Identification is an assertion of who someone is or what something is.
2. Authentication (认证)  
   Authentication is the act of verifying a claim of identity.
3. Authorization (授权)  
   After a person, program or computer has successfully been identified and authenticated then it must be determined what informational resources they are permitted to access and what actions they will be allowed to perform.

<details open>

或：
1. 身份认证：主客体间的**双向**认证
2. 控制策略的制定和实现
3. 审计
</details>

Access control models[^access-wiki]:
- [Multilevel Security](Access%20Control/Multilevel%20Security.md)
  - [Mandatory Access Control](Access%20Control/Mandatory%20Access%20Control.md)
- Discretionary/Mandatory
  - Discretionary Access Control (DAC, 自主访问控制)  
    In DAC, the data owner determines who can access specific resources. For example, a system administrator may create a hierarchy of files to be accessed based on certain permissions.
  - [Mandatory Access Control](Access%20Control/Mandatory%20Access%20Control.md) (MAC, 强制访问控制)  
    In MAC, users do not have much freedom to determine who has access to their files. For example, security clearance of users and classification of data (as confidential, secret or top secret) are used as security labels to define the level of trust.
- X-Based Access Control (XBAC)  
  - Attribute-based Access Control (ABAC)  
    An access control paradigm whereby access rights are granted to users through the use of policies which evaluate attributes (user attributes, resource attributes and environment conditions).
  - Graph-based Access Control (GBAC)  
    Compared to other approaches like RBAC or ABAC, the main difference is that in GBAC access rights are defined using an organizational query language instead of total enumeration.
  - History-Based Access Control (HBAC)  
    Access is granted or declined based on the real-time evaluation of a history of activities of the inquiring party, e.g. behavior, time between requests, content of requests. For example, the access to a certain service or data source can be granted or declined on the personal behavior, e.g. the request interval exceeds one query per second.
  - History-of-Presence Based Access Control (HPBAC)  
    Access control to resources is defined in terms of presence policies that need to be satisfied by presence records stored by the requestor. Policies are usually written in terms of frequency, spread and regularity. An example policy would be "The requestor has made k separate visitations, all within last week, and no two consecutive visitations are apart by more than T hours."
  - Identity-Based Access Control (IBAC)  
    Using this network administrators can more effectively manage activity and access based on individual needs.
  - Lattice-Based Access Control (LBAC)  
    A lattice is used to define the levels of security that an object may have and that a subject may have access to. The subject is only allowed to access an object if the security level of the subject is greater than or equal to that of the object.
  - Organization-Based Access Control (OrBAC)  
    OrBAC model allows the policy designer to define a security policy independently of the implementation.
  - Responsibility Based Access Control  
    Information is accessed based on the responsibilities assigned to an actor or a business role.
  - Role-Based Access Control (RBAC)  
    RBAC allows access based on the job title. RBAC largely eliminates discretion when providing access to objects. For example, a human resources specialist should not have permissions to create network accounts; this should be a role reserved for network administrators.
  - Rule-Based Access Control (RAC)  
    RAC method, also referred to as Rule-Based Role-Based Access Control (RB-RBAC), is largely context based. Example of this would be allowing students to use labs only during a certain time of day; it is the combination of students' RBAC-based information system access control with the time-based lab access rules.

## [Trusted Computing](Trusted%20Computing/README.md)

[^wiki]: [Information security - Wikipedia](https://en.wikipedia.org/wiki/Information_security)
[^access-wiki]: [Access control - Wikipedia](https://en.wikipedia.org/wiki/Access_control#Computer_security)
[^upkeep]: [What is the difference between availability and reliability?](https://www.upkeep.com/learning/availability-vs-reliability)  
