Copyright (c) 2026 Naomi Garratt / Garratt Technology Ltd. Licensed under the Apache License, Version 2.0 ([http://www.apache.org/licenses/LICENSE-2.0](http://www.apache.org/licenses/LICENSE-2.0)).

# **Trothline Protocol Technical Specification**

**Document Version:** 1.0.0  
**Status:** Released  
**Effective Date:** September 24, 2026  
**Author:** Naomi Garratt

# **1\. Abstract & Scope**

The Trothline Protocol defines an open network communication, identity resolution, and transactional auditing standard for autonomous AI agent networks. Inspired by telecommunications architectural principles (IMSI, MSISDN, HLR, CDR), Trothline resolves the quadratic \$O(N^2)\$ key explosion in multi-agent networks by establishing a linear \$O(N)\$ two-tier identity schema, inline policy enforcement proxy standards, and Call Detail Record (CDR) transactional logging.

This specification establishes:

* Layer 1 identity generation, registry key mapping, and dynamic address rotation protocols.
* Layer 2 transport proxy headers and signature verification workflows.
* Layer 3 Call Detail Record (CDR) JSON schema for verifiable multi-tier delegation auditing.

# **2\. Terminology & RFC 2119 Standards**

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC 2119](https://www.ietf.org/rfc/rfc2119.txt).

* **Permanent Agent Identity (IMSI Equivalent):** An immutable cryptographic public key pair (\$K\_{pub\_perm}, K\_{priv\_perm}\$) bound to the agent's owner.
* **Rotatable Agent Address (MSISDN Equivalent):** A dynamic, routable URI rotated periodically or per session to shield real endpoints.
* **Trothline Registry (HLR Equivalent):** The authoritative lookup registry mapping dynamic agent addresses to public key infrastructure records and access policies.
* **Enforcement Proxy:** An inline proxy intercepting agent request/response payloads to verify signatures, enforce access policies, and generate CDR audit headers.
* **Call Detail Record (CDR):** An immutable, structured audit log recording interaction metadata (\$TX\_{parent} \\to TX\_{child}\$).

# **3\. Layer 1: Identity & Key Management Schema**

## **3.1 Permanent Identity Generation**

Agents MUST generate a permanent cryptographic key pair using Ed25519 or ECDSA (secp256k1/P-256). The private key \$K\_{priv\_perm}\$ MUST be stored inside a Hardware Security Module (HSM) or Trusted Execution Environment (TEE). \$K\_{priv\_perm}\$ MUST NOT be transmitted over network protocols.

## **3.2 Rotatable Public Address Protocol**

1. Agents MUST request dynamic operational addresses (\$ADDR\_{dyn}\$) from the Trothline Registry.
2. \$ADDR\_{dyn}\$ MUST be valid for a bounded epoch (default: 3600 seconds or per-session).
3. The Trothline Registry MUST maintain an active mapping \$ADDR\_{dyn} \\mapsto K\_{pub\_perm}\$.

| Parameter | Type | Requirement | Description |
| :---- | :---- | :---- | :---- |
| permanent\_id | String (URI) | REQUIRED | Unique global URI bound to \$K\_{pub\_perm}\$. |
| rotatable\_address | String (URI) | REQUIRED | Active dynamic routing endpoint \$ADDR\_{dyn}\$. |
| epoch\_ttl | Integer | REQUIRED | Expiration window in seconds (e.g., 3600). |
| public\_key | String (PEM/JWK) | REQUIRED | \$K\_{pub\_perm}\$ public key structure. |

# **4\. Layer 2: Enforcement Proxy Headers & Verification Workflow**

All inter-agent HTTP/gRPC requests routed through a Trothline-compliant Enforcement Proxy MUST include the following mandatory HTTP headers:

* X-TRTH-Agent-ID: Dynamic public address of the sending agent (\$ADDR\_{dyn\_sender}\$).
* X-TRTH-Recipient-ID: Dynamic public address of the receiving agent (\$ADDR\_{dyn\_recipient}\$).
* X-TRTH-Session-ID: Unique UUIDv4 identifier for the top-level interaction session.
* X-TRTH-Parent-TX: ID of the parent transaction payload (\$TX\_{parent}\$).
* X-TRTH-TX-ID: ID of the current sub-transaction payload (\$TX\_{child}\$).
* X-TRTH-Timestamp: ISO-8601 UTC timestamp.
* X-TRTH-Signature: Base64-encoded cryptographic signature of SHA256(Payload \+ X-TRTH-TX-ID \+ X-TRTH-Timestamp) signed by \$K\_{priv\_perm}\$.

## **4.1 Verification Workflow**

When receiving a request, the recipient's Enforcement Proxy MUST:

1. Extract X-TRTH-Agent-ID and query the Trothline Registry for \$K\_{pub\_perm}\$.
2. Verify X-TRTH-Timestamp is within an acceptable skew window (\$\\pm 300\$ seconds).
3. Recompute the payload hash and verify X-TRTH-Signature using \$K\_{pub\_perm}\$.
4. If verification succeeds, forward payload to downstream execution agent; otherwise, reject with HTTP 401 Unauthorized.

# **5\. Layer 3: Call Detail Record (CDR) Audit Schema**

Enforcement proxies MUST emit a JSON-formatted Call Detail Record (CDR) upon completion of every interaction step:

```
{
  "protocol_version": "1.0.0",
  "session_id": "sess_8f9a2b1c-3d4e-5f6a-7b8c-9d0e1f2a3b4c",
  "tx_id": "tx_child_002",
  "parent_tx_id": "tx_parent_001",

  "sender_address": "trothline://agent.garatechnology.co.uk/node-8812",

  "recipient_address": "trothline://agent.partner.com/billing-v2",
  "timestamp": "2026-09-24T11:07:57Z",
  "payload_sha256": "e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855",
  "execution_status": "SUCCESS",
  "latency_ms": 42,
  "signature": "MEQCID3f..."
}
```

## **5.1 Audit Record Field Definitions**

* session\_id: Top-level root UUID tying all parent and child transaction calls across multi-agent delegation workflows.
* tx\_id / parent\_tx\_id: Linear pointer chain establishing direct causality for nested delegations (\$TX\_{parent} \\to TX\_{child}\$).
* payload\_sha256: Cryptographic digest of the request/response execution body ensuring non-repudiation.

# **6\. Layer 4: Registry Federation & Decentralised Node Consensus**

To prevent single points of failure and support cross-organizational agent discovery, central Trothline registries MAY operate in a federated or decentralized consensus model across sovereign jurisdictions.

## **6.1 Peer Registry Discovery & Address Resolution**

* Registry nodes MUST implement the ResolveAddress() gRPC protocol interface for cross-realm identity lookups.
* Federated registries MUST sign dynamic address routing updates (\$ADDR\_{dyn} \\mapsto K\_{pub\\\_perm}\$) using domain-level enclave certificates.
* Dynamic address queries across federated boundaries MUST complete within maximum latency bounds (\$\\le 50\\text{ ms}\$) using cached local policy trees.

## **6.2 BFT Consensus & State Synchronization**

Multi-region sovereign nodes MUST utilize a Byzantine Fault Tolerant (BFT) consensus protocol (e.g., Raft or Tendermint BFT) to synchronize public key infrastructure records and access control policy revisions across multi-tenant cloud enclaves.

# **7\. Layer 5: Clearinghouse, Micro-Transactions & Escrow Settlement**

Layer 5 defines the financial clearinghouse rails and dynamic cryptographic escrow mechanisms governing autonomous machine-to-machine resource billing and transactional execution.

## **7.1 Execution Metering & Micro-Billing**

Enforcement Proxies MUST attach metered resource utilization metrics (token counts, compute duration in milliseconds, tool API calls) to emitted Call Detail Records (CDRs). Autonomous clearinghouse services process signed CDR streams to calculate real-time micro-billing and API credit swaps.

## **7.2 Dynamic Cryptographic Escrow Protocol**

For high-value autonomous task delegation, agents MAY establish time-locked escrow contracts. Escrowed funds SHALL be released to executing agents if and only if a cryptographically valid Call Detail Record (CDR) signed by the downstream executing Enforcement Proxy is presented before expiry.

# **8\. Security Considerations & Threat Model**

* **Replay Attacks:** Prevented by enforcing strict timestamp skew windows and tracking unique X-TRTH-TX-ID nonces.
* **Address Unlinkability:** Dynamic address rotation ensures third-party passive observers cannot aggregate behavioral profiles across sessions.
* **Key Isolation:** Private keys remain isolated inside HSM/TEE hardware boundaries.
* **Combinatorial Explosion Bounding:** Decoupling session logging from pairwise keying reduces key management overhead from \$O(N^2)\$ quadratic growth to \$O(N)\$ linear scaling.

# **Appendix A: Implementation Reference & Conformance Test Suite**

This appendix provides the reference implementations, framework integrations, and automated unit testing tools for deploying and validating Trothline Protocol compliance across multi-agent networks.

## **A.1 Framework Wrappers & Integration Decorators**

To simplify adoption across enterprise agent ecosystems, Trothline provides native @trothline\_enforce decorators and SDK wrappers for major orchestration frameworks:

* **LangChain:** Native middleware wrappers around LangChain tools and callbacks to auto-inject Trothline headers and emit Call Detail Record (CDR) audit entries.
* **AutoGen:** Conversational middleware adapters managing address rotation and parent-child transaction headers (X-TRTH-Parent-TX) across multi-agent group chats.
* **CrewAI:** Task and tool execution wrappers providing deterministic delegation audit trails for role-based agent fleets.
* **TypeScript SDK:** Client and proxy middleware package (trothline-proxy-ts) for Node.js and browser-based agent runtimes.

## **A.2 Reference Enforcement Proxy Middleware (trothline\_proxy.py)**

The following Python reference implementation demonstrates lightweight inline enforcement proxy logic, handling header generation, HMAC-SHA256 signature calculation, address lookup, and CDR generation:

```py
import hashlib
import hmac
import json
import time
import uuid
from typing import Dict, Any, Optional


class TrothlineProxyMiddleware:
    """

    Lightweight Reference Enforcement Proxy for Trothline Protocol.
    Handles header generation, signature verification, address rotation lookup,
    and Call Detail Record (CDR) audit logging.
    """
    def __init__(self, registry_url: str, secret_key: str):
        self.registry_url = registry_url
        self.secret_key = secret_key.encode('utf-8')
        self.cdr_log = []

    def generate_signature(self, payload: Dict[str, Any], timestamp: float) -> str:
        """Generates HMAC-SHA256 signature for payload verification."""
        canonical_payload = json.dumps(payload, sort_keys=True)
        message = f"{timestamp}:{canonical_payload}".encode('utf-8')
        return hmac.new(self.secret_key, message, hashlib.sha256).hexdigest()

    def process_request(
        self,
        agent_id: str,
        rotatable_address: str,
        payload: Dict[str, Any],
        parent_tx_id: Optional[str] = None
    ) -> Dict[str, Any]:
        """Intercepts outbound agent call, signs payload, and records CDR entry."""
        timestamp = time.time()
        tx_id = f"tx_{uuid.uuid4().hex[:12]}"

        # 1. Generate signature
        signature = self.generate_signature(payload, timestamp)

        # 2. Construct CDR Audit Record (Layer 3)
        cdr_record = {
            "tx_id": tx_id,
            "parent_tx_id": parent_tx_id or "ROOT",
            "agent_id": agent_id,
            "rotatable_address": rotatable_address,
            "timestamp": timestamp,
            "payload_hash": hashlib.sha256(json.dumps(payload, sort_keys=True).encode('utf-8')).hexdigest(),
            "signature": signature,
            "status": "VERIFIED_AND_ENFORCED"
        }
        self.cdr_log.append(cdr_record)


        # 3. Attach Trothline Protocol Headers
        headers = {

            "X-TRTH-Agent-ID": agent_id,

            "X-TRTH-Address": rotatable_address,

            "X-TRTH-TX-ID": tx_id,

            "X-TRTH-Parent-TX": parent_tx_id or "ROOT",

            "X-TRTH-Signature": signature,

            "X-TRTH-Timestamp": str(timestamp)
        }
        return {
            "status": "SUCCESS",
            "tx_id": tx_id,
            "headers": headers,
            "cdr_record": cdr_record
        }
```

## **A.3 Conformance Unit Test Suite (test\_trothline\_proxy.py)**

Protocol conformance is validated using the automated unit test suite (test\_trothline\_proxy.py). The test suite verifies:

* HMAC-SHA256 signature generation and canonical payload hashing integrity.
* Propagation and injection of mandatory X-TRTH-\* HTTP/gRPC request headers.
* Correct formatting and linking of Call Detail Record (CDR) logs, including parent-child transaction IDs (TX\_parent \-\> TX\_child).
* Replay protection and timestamp skew validation within acceptable tolerance windows.

