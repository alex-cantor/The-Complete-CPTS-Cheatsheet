# **Penetration Testing Processes**
![PT-Process.jpg](Aspose.Words.b9cc7ec1-672e-42a8-a75f-f11afbda886f.001.jpeg) 

For a general list of useful pentesting tools click here:

![image-20240923-000336.png](Aspose.Words.b9cc7ec1-672e-42a8-a75f-f11afbda886f.002.png) 

Then click the page below:

![image-20240923-004803.png](Aspose.Words.b9cc7ec1-672e-42a8-a75f-f11afbda886f.003.png) 

[Penetration Testing Cheat Sheet - Not completed, but not terrible](https://github.com/curtishoughton/Penetration-Testing-Cheat-Sheet/tree/master) (Alex)

Here are initial notes I found useful while completing this module of the certification. Please add on to it, and learn from it.

**Overview**

**Types of Penetration Tests**

- Blackbox
  - Minimal
  - Only essential information disclosed
    - IP addresses
    - Domains
- Greybox
  - Extended
  - Slightly more information disclosed
    - IP addresses
    - Domains
    - Hostnames
    - Subnets
- Whitebox
  - Maximum
  - All information disclosed
    - Provided internal view of the structure
      - Allows better attack preperation
    - Possible provided:
      - Detailed configurations
      - Admin creds
      - Webapp source code
- Red-Teaming
  - May be combined with any of above
  - Includes physical testing and/or social engineering
- Purple-Teaming
  - May be combined with any of above
  - Done in coordination with blue teamers (defenders)

**Testing Environments (any combination may be tested)**

- Network
- Web App
- Mobile
- API
- Thick Clients
- IoT
- Cloud
- Source Code
- Physical Security
- Employees
- Hosts
- Server
- Security Policies
- Firewalls
- IDS/IPS (Intrusion Defense System / Intrusion Prevention System)

**Laws and Regulations**

**Laws to keep in mind (US)**

- Computer Fraud and Abuse Act (CFAA)
  - Prohibits accessing a computer without authorization
    - Considered a criminal offense
- Digital Miillennium Copyright Act (DMCA)
  - Prohibits circumventing technological measures to protect copyright works
  - Includes digital locks, encryption, and authentication protocols
- Electronic Communications Privacy Act (ECPA)
  - This law makes it unlawful to intercept, access, monitor, or store communications [through the internet] without one or both parties consent
- Health Insurance Portability and Accountability Act (HIPAA)
  - Health data must be kept secure and encrypted
- Children’s Online Privacy Protection Act (COPPA)
  - Applies to children under the age of 13
  - Must not collect, use, or disclose personal information
- **Precautionary Measures during Penetration Tests**
- Obtain written consent from the authorized representative of the computer or network being tested
- Conduct testing within the scope of the consent obtained only and respect any limitation specified
- Take measures to prevent causing damage to the systems or networks being tested
- Do not access, use or disclose personal data or any other information obtained during the testing without permission
- Do not intercept electronic communications without the consent of one of the parties to the communication
- Do not conduct testing on systems or networks that are covered by HIPAA without proper authorization

**Penetration Testing Process**

- **Stages of Penetration Testing**
1. Pre-Engagement
   1. Steps to Pre-Engagement
      1. **C**reate necessary docs
      1. **A**sk clarifying questions
      1. **D**iscuss assessment objectives
   1. Types of **Non-Disclosure Agreements (NDAs)**
      1. Unilateral NDA
         1. Obliges only one party to maintain confidentiality
         1. Allows other party to share information with third parties
      1. Bilateral NDA
         1. Both parties obliged to keep information confidential
         1. Most common type of NDA
         1. Protects work of penetration testers
      1. Multilateral NDA
         1. Commitment to confidentiality by more than two parties
         1. If we conduct a penetration test for a cooperative network, all parties involved must sign this document
   1. Company Members (potentially) authorized to hire pen testers:
      1. CEO
      1. CTO
      1. CISO
      1. CSO
      1. CRO
      1. CIO
      1. VP of Internal Audit
      1. Audit Manager
      1. VP or Director of IT / Information Security
   1. Necessary Documents
      1. Non-Disclosure Agreement (NDA)
         1. Timing: After Initial Contact
      1. Scoping Questionnaire
         1. Timing: Before Pre-Engagement Meeting
      1. Scoping Document
         1. Timing: During Pre-Engagement Meeting
      1. Penetration Testing Proposal (Contract / Scope of Work (SoW))
         1. Timing: During Pre-Engagement Meeting
      1. Rules of Engagement (RoE)
         1. Timing: Before the Kick-Off Meeting
      1. Contractors Agreement (Physical Assessments)
         1. Timing: Before the Kick-Off Meeting
      1. Reports
         1. Timing: During and After the Conducted Penetration Test
1. Information Gathering
   1. Investigate company’s existing website
   1. Identify technologies in use
   1. Learn how the webapps function
   1. Types of Information Gathering:
      1. Open Source Intelligence (OSINT)
      1. Infrastructure Enumeration
      1. Service Enumeration
      1. Host Enumeration
1. Vulnerability Assessment
   1. Look for known vulnerabilities
   1. Investigate questionable features
      1. QUESTION EVERYTHING
   1. Analysis Types
      1. Descriptive
      1. Diagnostic
      1. Predictive
      1. Prescriptive
1. Exploitation
   1. Prepare:
      1. Exploit code
      1. Tools
      1. Environment
   1. Test web server for potential vulnerabilities
   1. Once one or two vulnerabilities have been discovered, you must assess which attack to prioritize
      1. Probability of Success (CVSS Score)
      1. Complexity
      1. Probability of Damage
1. Post-Exploitation
   1. Upon exploitation, gather information on the web server
   1. Sensitive information may allow for privilege escalation
1. Lateral Movement
   1. Move through network (assuming in scope)
   1. Access other hosts and servers
1. Proof-of-Concept
   1. Create a proof-of-concept that proves these vulnerabilities exist
   1. Potentially automate individual steps that trigger these vulnerabilities
1. Post Engagement
   1. Present documentation in formal report deliverable
   1. May hold report walkthrough to clarify anything about testing or results and provide needed support in remediation
