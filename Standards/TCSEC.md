# Trusted Computer System Evaluation Criteria
**Trusted Computer System Evaluation Criteria (TCSEC, 可信计算机系统评估标准)** is a United States Government Department of Defense (DoD) standard that sets basic requirements for assessing the effectiveness of computer security controls built into a computer system. The TCSEC was used to evaluate, classify, and select computer systems being considered for the processing, storage, and retrieval of sensitive or classified information.[^wiki]

The TCSES defines following divisions:
- D: Minimal protection
- [C: Discretionary protection](#c-discretionary-protection)
  - C1: Discretionary security protection
  - C2: Controlled access protection
- [B: Mandatory protection](#b-mandatory-protection)
  - B1: Labeled security protection
  - B2: Structued protection
  - B3: Security domains
- [A: Verified protection](#a-verified-protection)
  - A1: Verified design

## C: Discretionary protection
- C1: Discretionary security protection
  - Identification and authentication
  - Separation of users and data
  - DAC (Discretionary Access Control) capable of enforcing access limitations on an individual basis
  - Required System Documentation and user manuals
- C2: Controlled Access Protection
  - More finely grained DAC
  - Individual accountability through login procedures
  - Audit trails
  - Object reuse
  - Resource isolation

## B: Mandatory protection
- B1: Labeled security protection
  - **Informal** statement of the security policy model
  - Data sensitivity labels
  - MAC (Mandatory Access Control) over **selected** subjects and objects
  - Label exportation capabilities
  - Some discovered flaws must be removed or otherwise mitigated
  - Design specifications and verification
- B2: Structured protection
  - Security policy model clearly defined and **formally** documented
  - DAC and MAC enforcement extended to **all** subjects and objects
  - **Covert storage channels** are analyzed for occurrence and bandwidth
  - Carefully structured into protection-critical and non-protection-critical elements
  - Design and implementation enable more comprehensive testing and review
  - Authentication mechanisms are strengthened
  - Trusted facility management is provided with administrator and operator segregation
  - Strict configuration management controls are imposed
  - Operator and Administrator roles are separated.
- B3: Security domains
  - Satisfies reference monitor requirements
  - Structured to exclude code not essential to security policy enforcement
  - Significant system engineering directed toward minimizing complexity
  - Security administrator role defined
  - Audit security-relevant events
  - Automated imminent intrusion detection, notification, and response
  - Trusted path to the **Trusted Computing Base (TCB)** for the user authentication function
  - Trusted system recovery procedures
  - **Covert timing channels** are analyzed for occurrence and bandwidth

## A: Verified protection
- A1: Verified design
  - Functionally identical to B3
  - Formal design and verification techniques including a formal top-level specification
  - Formal management and distribution procedures
- Beyond A1
  - System Architecture demonstrates that the requirements of self-protection and completeness for reference monitors have been implemented in the TCB.
  - Security Testing automatically generates test-case from the formal top-level specification or formal lower-level specifications.
  - Formal Specification and Verification is where the TCB is verified down to the source code level, using formal verification methods where feasible.
  - Trusted Design Environment is where the TCB is designed in a trusted facility with only trusted (cleared) personnel.

Examples of A1-class systems are Honeywell's SCOMP, Aesec's GEMSOS, and Boeing's SNS Server. Two that were unevaluated were the production LOCK platform and the cancelled DEC VAX Security Kernel.

[^wiki]: [Trusted Computer System Evaluation Criteria - Wikipedia](https://en.wikipedia.org/wiki/Trusted_Computer_System_Evaluation_Criteria)