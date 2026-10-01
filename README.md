Copyright (c) 2026 Naomi Garratt / Garratt Technology Ltd. Licensed under the Apache License, Version 2.0 ([http://www.apache.org/licenses/LICENSE-2.0](http://www.apache.org/licenses/LICENSE-2.0)).

**Document Version:** 1.0.0  
**Effective Date:** September 24, 2026  
**Copyright:** Copyright (c) 2026 Naomi Garratt / Garratt Technology Ltd  
**License:** Licensed under the Apache License, Version 2.0 ([http://www.apache.org/licenses/LICENSE-2.0](http://www.apache.org/licenses/LICENSE-2.0))

# **Trothline Protocol: identity, routing and delegation auditing for autonomous AI agents**

**The Telecom-Inspired Open Standard for Global Autonomous AI Agent Identity, Routing, and Delegation Auditing.**

# **Overview**

Trothline Protocol adapts proven telecommunications architectural patterns (IMSI/MSISDN/HLR/CDR) to autonomous AI agent networks. It solves the *N × M* credential and identity state explosion by reducing key management complexity to *O(N)* linear scaling, decoupling persistent cryptographic identities from dynamic operational endpoints, and generating immutable Call Detail Record (CDR) audit trails.

# **Key Architectural Layers**

* **Layer 1: Two-Tier Identity Model (IMSI/MSISDN/HLR Equivalent)**
  * *Permanent Cryptographic Identity (Kpriv\_perm):* Secured in HSM/TEE enclaves; never exposed operationally.
  * *Rotatable Public Address (ADDRdyn):* Dynamic, routable address rotated periodically or per-session to prevent endpoint tracking.
  * *Central Trothline Registry:* Authoritative lookup resolving active public addresses to persistent identities and access policies.
* **Layer 2: Inline Policy Enforcement Proxy**
  * Intercepts outbound/inbound agent traffic, verifies HMAC-SHA256 signatures, validates schemas, and enforces access control rules prior to downstream tool execution.
* **Layer 3: Transactional Delegation Auditing (CDR Model)**
  * Generates linear, immutable Call Detail Record (CDR) logs with parent-child transaction headers (*TXparent* → *TXchild*) to audit multi-hop agent delegation chains.

# **Quick Start (Python Reference Proxy)**

## **Installation**

```sh

pip install trothline-proxy

```

## **Basic Usage**

```py

from trothline_proxy import TrothlineProxyMiddleware


# Initialize proxy with Trothline Registry URL and signing secret

proxy = TrothlineProxyMiddleware(



    registry_url="https://registry.trothline.org",
    secret_key="your_secure_secret_key"
)

# Intercept and sign an agent delegation payload
result = proxy.process_request(
    agent_id="agent_imsi_101",
    rotatable_address="msisdn_rot_alpha_001",
    payload={"action": "execute_tool", "tool": "data_parser", "params": {"file_id": "doc_101"}}
)

print("Headers:", result["headers"])
print("CDR Audit Entry:", result["cdr_record"])
```

# **Running Conformance Tests**

To run the automated test suite and verify proxy implementation compliance:

```sh

pytest test_trothline_proxy.py

```

# **Repository Structure**

* spec/: RFC 2119-compliant Trothline Protocol Technical Specification.
* src/trothline\_proxy/: Reference Enforcement Proxy implementation in Python.
* tests/: Conformance unit test suite.
* docs/: Whitepaper, governance docs, and architecture visualizer.

# **Framework Integrations & Middleware**

Trothline Protocol provides native framework integration wrappers and decorators to streamline adoption across enterprise multi-agent ecosystems:

* **LangChain Integration:** Native middleware wrappers and tool callbacks automatically inject X-TRTH-\* parent-child transaction headers and emit Call Detail Record (CDR) audit entries across multi-step execution chains.
* **AutoGen Integration:** Conversational middleware adapters manage dynamic address rotation and parent-child header propagation (X-TRTH-Parent-TX) across nested group chat conversations.
* **CrewAI Integration:** Task and tool execution wrappers leverage the **@trothline\_enforce** decorator to deliver automated payload signing and deterministic delegation audit trails for role-based agent fleets.

# **Specification & Governance**

* **Specification Text:** Licensed under [Creative Commons Attribution 4.0 International (CC-BY 4.0)](https://creativecommons.org/licenses/by/4.0/).
* **Reference Implementation:** Licensed under the [Apache License, Version 2.0](https://www.apache.org/licenses/LICENSE-2.0) (includes standard copyright headers and NOTICE requirements).
* **Patent Policy:** Royalty-Free (RF) Patent Commitment (W3C-style) protecting all adopters and implementers.

# **Community & Contributing**

We welcome contributions from framework maintainers, enterprise security architects, and AI safety researchers\! Please review our [Contribution Guidelines (CONTRIBUTING.md)](https://docs.google.com/document/d/1OnLPD88PPRKfuyz27aB8a5Vy4bHBlBxTMZEHVhlKX-o/edit) before submitting Pull Requests or RFC proposals.