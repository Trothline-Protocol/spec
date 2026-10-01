# **Trothline Protocol \- Governance Structure & Intellectual Property Policy**

**Document Version:** 1.0.0  
**Effective Date:** September 24, 2026  
Copyright (c) 2026 Naomi Garratt / Garratt Technology Ltd  
Licensed under the Apache License, Version 2.0 ([http://www.apache.org/licenses/LICENSE-2.0](http://www.apache.org/licenses/LICENSE-2.0))

# **Summary Matrix**

| Governance Dimension | Specification & Schemas (CC-BY 4.0) | Reference Code & SDKs | Essential Patent Claims |
| :---- | :---- | :---- | :---- |
| **Licensing Model** | Creative Commons CC-BY 4.0 | Apache License 2.0 | Royalty-Free (RF) Non-Assertion |
| **Primary Steward** | Technical Steering Committee (TSC) | Open Source Maintainers / TSC | Industry Alliance / Consortium (Phase 4\) |
| **Contribution Path** | Public RFC Process (trothline-protocol/spec) | GitHub Code Reviews / CI | Contributor License Agreement (CLA) |

# **1\. Mission and Open Standard Philosophy**

The Trothline Protocol is established as an open, telecom-inspired royalty-free standard to foster global identity resolution, dynamic address rotation, and tamper-evident Call Detail Record (CDR) delegation auditing across multi-agent networks. No single commercial entity shall maintain exclusive control over the specification.

# **2\. Intellectual Property & Licensing Framework**

## **2.1 Specification Licensing**

The Trothline Protocol Specification text and schemas are published under the **Creative Commons Attribution 4.0 International License (CC-BY 4.0)**. Anyone is free to copy, redistribute, modify, and build upon the specification text provided attribution is given.

## **2.2 Reference Code & SDK Licensing**

All official reference implementations (e.g., trothline\_proxy.py), enforcement proxy SDKs, and test suites (test\_trothline\_proxy.py) maintained by the Trothline Working Group are licensed under the Apache License, Version 2.0 with structured attribution via the NOTICE file.

## **2.3 Royalty-Free (RF) Patent Commitment**

To ensure that implementers can adopt the Protocol without legal friction or patent infringement concerns:

* **Royalty-Free Non-Assertion Pledge:** All contributors to the Trothline Protocol specification irrevocably commit to grant implementers a non-exclusive, royalty-free, worldwide patent license under their Essential Claims to make, use, sell, offer for sale, or import implementations of the specification.
* **Defensive Termination:** The patent commitment shall automatically terminate with respect to any party that files a patent infringement lawsuit against another implementer alleging that a Trothline Protocol-compliant implementation infringes its patent rights.

# **3\. Governance Structure**

## **3.1 Phase 1: Technical Steering Committee (Current Model)**

During the initial launch phase (Months 1–6), governance is managed by the **Trothline Technical Steering Committee (TSC)**, comprising founding architects, enterprise security advisors, and lead open-source maintainers.

### **3.1.1 Membership and Onboarding**

Individuals and organizations may join the Technical Steering Committee (TSC) and participate in the future foundation through a structured onboarding process based on demonstrated commitment to the protocol. Criteria and onboarding steps include:

* **Active Contributions:** Applicants must demonstrate sustained technical or operational contributions, such as authoring or reviewing RFCs, maintaining reference SDKs, or supporting open-source tools.
* **Nomination Process:** Candidates must be nominated by an existing TSC member or submit a formal request highlighting their track record and proposed areas of stewardship.
* **TSC Approval:** Membership requests are reviewed by the TSC and require pre-approval by a simple majority vote to formally onboard the candidate or organization and final approval by Naomi Garratt, Chief Architect of the Trothline Protocol.
* **Seat Cap Recommendation:** To maintain operational agility, it is recommended to cap the TSC at 7 active voting members.

## **3.2 Phase 2: Transition to Independent Foundation**

In Phase 4, the Trothline Protocol will transition governance to an independent industry alliance/consortium to ensure multi-stakeholder oversight long-term. Negotiations and discussions for transitioning to the appropriate umbrella or alliance will occur during Phase 2 and Phase 3\.

# **4\. Specification Change Process (RFC Lifecycle)**

Proposed changes to the specification MUST follow the formal RFC process:

1. **Draft Stage (RFC Submission):** Proposed via a Pull Request to the trothline-protocol/spec GitHub repository or agentexchangeprotocol.org using the standard RFC Markdown template.
2. **Review Stage (Public Comment):** Minimum 14-day public review period for community feedback and security analysis.
3. **Ratification Stage:** Requires approval by a simple majority vote of the Technical Steering Committee (e.g., a majority of active voting members) to be merged into the normative specification. In the event of a tied vote or deadlock among the Technical Steering Committee, the Chief Architect (Naomi Garratt) holds the final tie-breaking decision-making authority during Phase 1\.

# **5\. Code of Conduct**

All contributors and maintainers MUST adhere to the Contributor Covenant Code of Conduct v2.1 to foster an inclusive, respectful, and safe ecosystem.