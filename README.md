# ARC-1 — Agent Resource Contract Standard

**An open standard for the Agent Economy**

Version 1.2 — May 2026 | [Changelog](#changelog) | [Implementation Guide](https://github.com/arc1-standard/standard/blob/main/ARC-1_ImplementationGuide.md)

---

> *The analogy is direct: just as ERC-20 first standardized tokens and thereby enabled Uniswap, ARC-1 standardizes agent services and makes a "Uniswap for Agent Labor" possible.*

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [Problem Analysis](#2-problem-analysis)
3. [The Solution: ARC-1](#3-the-solution-arc-1)
4. [The Full Stack](#4-the-full-stack)
5. [Why This Is Genuinely New](#5-why-this-is-genuinely-new)
6. [Budget Management for Autonomous Agents](#6-budget-management-for-autonomous-agents)
7. [Interface Specification](#7-interface-specification)
8. [Identity & Attestation](#8-identity--attestation)
9. [Service Categories](#9-service-categories)
10. [Router Specification](#10-router-specification)
11. [SLA Conformance Verification](#11-sla-conformance-verification)
12. [Security](#12-security)
13. [Anti-Spam](#13-anti-spam)
14. [Cross-Chain](#14-cross-chain)
15. [Roadmap](#15-roadmap)
16. [Open Questions & Honest Risks](#16-open-questions--honest-risks)
17. [Conclusion](#17-conclusion)

- [Changelog](#changelog)

---

## 1. Executive Summary

ARC-1 is an open standard that makes agent services fungible — the missing primitive of the Agent Economy.

Today, AI agents can neither automatically discover services, nor compare them, nor pay for them trustlessly. x402 solves the payment UX problem, but not the missing fungibility. Sentiment analysis from provider A is not the same as sentiment analysis from provider B — without common interfaces, no router can make meaningful comparisons.

ARC-1 v1.2 defines seven core areas:

| Area | What it solves |
| --- | --- |
| Interface Specification | `describe()`, `quote()`, `execute()` as mandatory core functions |
| Quality Metrics | Standardized SLA declaration and conformance verification via Muon |
| Payment Interface | x402-compatible, chain-agnostic, escrow-supported |
| Budget Management | Hierarchical delegation, refund flow, payment channels without a gatekeeper |
| Error Handling | Result states, penalty formula, trustless dispute resolution via Muon |
| Identity & Attestation | DID-based provider identity, Muon certification, anti-spam |
| Governance & Versioning | Three-phase rollout, MAJOR.MINOR.PATCH, royalty-free |

The stack above — routers, marketplaces, DeFi-like protocols — can only emerge once this primitive exists. ARC-1 is that proposal.

---

## 2. Problem Analysis

### 2.1 The Starting Point: x402 and Its Limits

The HTTP-402 protocol ("Payment Required") enables native micropayments in HTTP requests. Projects like Meridian ($MRDN) build payment rails for AI agents on top of it. The idea is real — but x402 alone only solves a UX problem: payment without manual integration.

The fundamental weaknesses of x402 as a standalone solution:

- **Stateless** — each payment is isolated, no memory across transactions
- **EVM-only** — no Solana, no Cosmos, no chain-agnostic usage
- **No identity layer** — who is the agent provider? What quality do they deliver?
- **No dispute mechanism** — payment is gone regardless of quality
- **No budget management** — no native spending limit for autonomous agents

### 2.2 The Real Problem: Missing Fungibility

Uniswap works because 1 ETH = 1 ETH. Tokens are fungible. Labor is not — not yet.

**Sentiment analysis from provider A ≠ Sentiment analysis from provider B**

Without common interfaces, quality metrics, and pricing units, no router can make meaningful comparisons. This is the deeper problem that x402 does not solve.

### 2.3 The Missing Stack

| Layer | Description | Status today |
| --- | --- | --- |
| Intent Layer | Agent formulates goals, not endpoints | Non-existent |
| Routing Layer | Automatic selection of the best provider | Non-existent |
| **ARC-1 Standard** | **Fungibility of agent services** | **← This gap** |
| Verification Layer | Trustless quality checks (Muon) | Partial |
| Payment Rail | x402 / Meridian | Early, EVM-only |

---

## 3. The Solution: ARC-1

ARC-1 is not a protocol and not a token project. It is an open standard — comparable to W3C standards or the EIP process.

### 3.1 Interface Definition

```json
{
  "service_type": "sentiment_analysis",
  "input_schema": { "texts": ["string"] },
  "output_schema": { "scores": ["float -1..1"] },
  "pricing_unit": "per_1000_tokens",
  "accepted_tokens": ["USDC"],
  "supported_chains": ["base", "solana", "polygon"]
}
```

### 3.2 Quality Metrics

- **Latency SLA** — maximum response time in ms
- **Conformance attestation** — verified via Muon for in-scope categories (see §11)
- **Uptime requirement** — minimum availability (e.g. 99.5%)
- **Confidence Score** — optional output field for self-reported quality

### 3.3 Payment Interface

- x402 compatibility as baseline
- Chain-agnostic declaration of supported payment rails
- Streaming payment option for long-running tasks
- Escrow flag: indicates whether conditional payment is supported

### 3.4 Identity & Attestation

- Provider DID (Decentralized Identifier)
- Muon attestation as compliance proof
- On-chain verifiable certification hash

---

## 4. The Full Stack

### 4.1 Architecture Overview

```
┌──────────────────────────────┐
│     INTENT LAYER             │  ← Agent formulates goals
├──────────────────────────────┤
│     ROUTING LAYER            │  ← Automatic provider selection
├──────────────────────────────┤
│     ARC-1 STANDARD           │  ← Fungibility / The primitive
├──────────────────────────────┤
│  MUON (Verification Layer)   │  ← Trustless conformance verification
├──────────────────────────────┤
│  x402 / MERIDIAN (Payment)   │  ← Execution rail
└──────────────────────────────┘
```

### 4.2 Role of Muon

- Muon nodes run conformance requests against ARC-1 endpoints (see §11 for scope)
- Nodes sign compliance proofs via TSS (Threshold Signature Scheme)
- Signatures are on-chain verifiable — decentralized certification without a central authority
- Historical performance data becomes available as a routing signal

### 4.3 Role of x402 / Meridian

x402 remains what it is: an elegant convenience layer for payment triggering in HTTP requests. In the ARC-1 stack, x402 is the execution layer — not the foundation.

> **Important:** ARC-1 does not depend on Meridian. Any x402-compatible payment system can serve as the rail.

---

## 5. Why This Is Genuinely New

### 5.1 Comparison with Existing Solutions

| Project | What it solves | What is missing |
| --- | --- | --- |
| Meridian / x402 | Payment UX for agents | Standard, routing, reputation |
| Muon | Trustless off-chain validation | No payment, no service standard |
| Chainlink | Price feed oracle | No agent labor standard |
| Virtuals Protocol | Agent tokenization | No interoperable payment interface |
| **ARC-1 (Proposal)** | **Fungibility of agent services** | **← Fills this gap** |

### 5.2 The ERC-20 Analogy

| ERC-20 World | ARC-1 World |
| --- | --- |
| Tokens are fungible | Agent services are fungible |
| `transfer()`, `approve()`, `balanceOf()` | `execute()`, `quote()`, `describe()` |
| Uniswap as router | Labor router as the next layer |
| DeFi Summer | Agent Economy |

---

## 6. Budget Management for Autonomous Agents

> *The central open question of ARC-1 v0.1, addressed in v0.2: How do you control the budget of autonomous agents that spawn sub-agents and can pay autonomously — without a central gatekeeper?*

### 6.1 The Problem

An autonomous agent has no credit card and no API key account. It has a wallet — a private key, USDC balance, the ability to pay on-chain. When it spawns sub-agents, an uncontrolled spending tree emerges:

```
Human
  └→ Agent A (50 USDC budget)
        └→ Agent B (how much?)
              └→ Agent C (how much?)
```

The solution: a smart contract as a trustless budget enforcer.

### 6.2 Delegation

```
Human → Contract: "50 USDC, max 3 levels"
Contract → Agent A: Allowance 50 USDC, depth 3
Agent A  → Contract: "delegate 10 USDC to Agent B, depth 2"
Contract → Agent B: Allowance 10 USDC, depth 2
```

```solidity
struct Allowance {
    address agent;        // Who may spend
    uint256 amount;       // Available budget
    uint256 maxDepth;     // Maximum sub-agent depth
    uint256 currentDepth; // Current depth in the tree
    address parent;       // For refund flow
    uint256 expires;      // Timeout
}
```

### 6.3 Refund Flow

Two triggers initiate automatic refund flow:

**Trigger 1 — Explicit:**

```
Agent B → Contract: done(), unused: 7 USDC
Contract → Agent A allowance: +7 USDC
```

**Trigger 2 — Timeout:**

```
expires: 24h
After 24h → Contract automatically returns budget to parent
```

### 6.4 Speed — Payment Channels

```
Human → Contract: open channel, 50 USDC deposit  (1x on-chain)

Agent pays 0.001 USDC  →  off-chain signature
Agent pays 0.001 USDC  →  off-chain signature
... 1000x ...

Agent → Contract: close channel, settle           (1x on-chain)
```

Off-chain signature:

```json
{
  "from": "Agent_Wallet",
  "to": "Provider",
  "amount": 0.001,
  "nonce": 1042,
  "signature": "0x..."
}
```

### 6.5 Summary

| Problem | Solution | Status |
| --- | --- | --- |
| Delegation | Hierarchical contract with `parent` + depth limit | Workable |
| Refund flow | Automatic via `done()` + timeout | Workable |
| Speed | Payment channels, off-chain settlement | Complex, solvable |
| Watchtower | Muon as decentralized channel monitor | Depends on Muon maturity |

---

## 7. Interface Specification

### 7.1 `describe()`

`GET /arc1/describe` — no input, full service description:

```json
{
  "service_type": "sentiment_analysis",
  "arc1_version": "1.2.0",
  "provider_did": "did:arc1:0x1234abcd",
  "input_schema": { "texts": ["string"] },
  "output_schema": { "scores": ["float -1..1"] },
  "pricing_unit": "per_1000_tokens",
  "accepted_tokens": ["USDC"],
  "supported_chains": ["base", "solana"],
  "sla": { "latency_ms": 500, "uptime": 99.5 },
  "compliance_level": "standard",
  "model": "deepseek/deepseek-chat-v3"
}
```

### 7.2 `quote()`

`POST /arc1/quote` — price for a concrete input:

```json
// Request
{ "input": { "texts": ["Hello world"] } }

// Response
{ "price": 0.001, "token": "USDC", "valid_for_seconds": 30 }
```

> `valid_for_seconds` is mandatory — prevents price manipulation between `quote()` and `execute()`.

### 7.3 `execute()`

`POST /arc1/execute` — service call with payment proof:

```json
// Request
{
  "input": { "texts": ["Hello world"] },
  "payment": {
    "channel_id": "0x...",
    "signature": "0x...",
    "amount": 0.001
  }
}

// Response
{
  "output": { "scores": [0.73] },
  "result_hash": "0x...",
  "provider_signature": "0x...",
  "timestamp": 1234567890
}
```

> `result_hash` and `provider_signature` are verified by Muon — without knowing the output content.

---

## 8. Identity & Attestation

### 8.1 DID Structure

```
did:arc1:0x1234abcd...
  ↑    ↑      ↑
  |    |      Address (wallet address)
  |    Method (arc1-specific)
  Schema
```

### 8.2 DID Document

```json
{
  "id": "did:arc1:0x1234abcd",
  "verificationMethod": [{
    "id": "did:arc1:0x1234abcd#key-1",
    "type": "EcdsaSecp256k1",
    "publicKeyHex": "0x..."
  }],
  "service": [{
    "type": "ARC1Service",
    "serviceEndpoint": "https://api.provider.com/arc1"
  }]
}
```

### 8.3 Two-Phase DID Rollout

| Phase | Method | Advantages | Disadvantages |
| --- | --- | --- | --- |
| Phase 1 | `did:web` | Simple, no registry needed | Domain-dependent |
| Phase 2 | `did:arc1` | Chain-agnostic, persistent, revocable | Requires own registry |

`did:arc1` uses Muon as a decentralized registry — nodes sign registrations via TSS.

### 8.4 What DID Solves

- **Persistence** — provider can change domain, reputation is retained
- **Verification** — provider signs output with DID key, Muon verifies without knowing the provider
- **Revocation** — compromised provider can be revoked on-chain, effective immediately

---

## 9. Service Categories

### 9.1 Initial Categories (v1.0)

| Category | Service Type | v1.0 Conformance Mechanism |
| --- | --- | --- |
| Analysis | `sentiment_analysis` | §11 polling |
| Analysis | `entity_extraction` | §11 polling |
| Analysis | `text_classification` | §11 polling |
| Analysis | `summarization` | §11 polling |
| Compute | `embedding_generation` | §11 polling |
| Compute | `translation` | §11 polling |
| Compute | `code_execution` | self-declaration + reputation |
| Compute | `image_generation` | self-declaration + reputation |
| Data | `document_parse` | §11 polling |
| Data | `web_fetch` | self-declaration + reputation |
| Data | `database_query` | self-declaration + reputation |
| Data | `price_feed` | self-declaration + reputation |

Categories marked **§11 polling** are subject to cryptographic conformance verification as defined in Section 11. Categories marked **self-declaration + reputation** rely on the provider's declared SLA in `describe()` and on signed user reports aggregated by routers (see §10). Both are valid v1.2 mechanisms; they differ in the strength of the trust assumption a router can make about provider claims.

### 9.2 Category Definition

```json
{
  "category": "sentiment_analysis",
  "version": "1.0",
  "input_schema": {
    "texts": ["string"],
    "max_length": 10000
  },
  "output_schema": {
    "scores": ["float -1..1"],
    "confidence": ["float 0..1"]
  },
  "conformance_mechanism": "section_11_polling",
  "pricing_unit": "per_1000_tokens"
}
```

The `conformance_mechanism` field is mandatory and MUST be one of `section_11_polling` or `self_declaration_reputation`. This makes the verification expectation explicit at the category-definition level rather than buried in cross-references.

> New categories are MINOR changes — no breaking change, no supermajority required. New categories MUST declare a `conformance_mechanism`. Governance follows the process in §16.4.

---

## 10. Router Specification

### 10.1 Router Flow

```
Agent formulates intent:
"I need sentiment_analysis, max 0.002 USDC, max 300ms"

Router:
1. Filters   → only Core-compliant providers
2. Matches   → only sentiment_analysis providers
3. Ranks     → by price, latency, reputation
4. Quotes    → calls quote() for top-3
5. Selects   → best provider
6. Returns   → endpoint + payment details to agent
```

### 10.2 Ranking Formula

```
Score = (Price score      × 0.4)
      + (Latency score    × 0.3)
      + (Reputation score × 0.3)
```

The weights shown are a v1.2 reference default. Routers MAY use different weights and MAY publish their weighting scheme. The constraint is that price, latency, and reputation MUST all be inputs to the score.

- **Price score** — derived from `quote()` responses for the candidate set. The cheapest provider receives 1.0, the most expensive 0.0, others scaled linearly.
- **Latency score** — for §11-polling categories, derived from the most recent signed conformance attestation (p95 latency vs. declared SLA). For other categories, from the provider's self-declared SLA in `describe()`, optionally adjusted by signed user reports. Routers SHOULD discount self-declared latency in the absence of corroborating user reports.
- **Reputation score** — for §11-polling categories, a combination of attested conformance (availability, schema compliance, latency adherence) over the last 30 days and signed user reports of service outcomes. For other categories, signed user reports only. Routers MUST publish whether their reputation score includes Muon attestations, user reports, or both.

### 10.3 Agent Request

```json
{
  "service_type": "sentiment_analysis",
  "constraints": {
    "max_price": 0.002,
    "max_latency_ms": 300,
    "min_reputation": 0.95,
    "accepted_tokens": ["USDC"],
    "chains": ["base", "solana"]
  },
  "preferences": { "priority": "price" }
}
```

### 10.4 Router Response

```json
{
  "provider_did": "did:arc1:0x1234",
  "endpoint": "https://api.provider.com/arc1",
  "quote": { "price": 0.001, "token": "USDC", "valid_for_seconds": 30 },
  "reputation": 0.98,
  "reputation_basis": "attestation+reports",
  "arc1_version": "1.2.0"
}
```

The `reputation_basis` field is mandatory. It MUST be one of `attestation+reports`, `attestation_only`, `reports_only`, or `self_declared`. This lets agents distinguish between strongly verified reputation and weakly verified reputation when making routing decisions.

### 10.5 Router Rollout

| Phase | Approach | Status |
| --- | --- | --- |
| Phase 1 | Open-source reference implementation | Actionable today |
| Phase 2 | Muon as decentralized router | Depends on Muon maturity |
| Phase 3 | Many independent router implementations | Result of Phase 1+2 |

---

## 11. SLA Conformance Verification

### 11.1 Scope and Boundaries

This section defines how ARC-1 verifies that a registered provider meets its declared service-level commitments. Verification covers externally observable properties only: latency, availability, and output-schema compliance.

This section does NOT cover adversarial quality benchmarking — i.e., verifying that a provider returns *correct* or *high-quality* answers as opposed to *well-formed* answers. The limitations of black-box quality verification are discussed explicitly in §11.7.

The distinction matters: the two problems require fundamentally different solutions, and conflating them produces standards that overpromise what they verify.

### 11.2 Muon as Decentralized Conformance Tester

```
Muon committee → Provider: execute() with valid input
Muon committee  measures: response time, availability, schema compliance
Muon committee  reaches consensus on observed metrics
Muon committee  signs TSS attestation: { provider_did, window, metrics }
Attestation     is anchored on-chain and consumable by routers
```

The Muon committee operates as a threshold-signature group over a Schnorr key. An attestation is valid if and only if a threshold of the committee has signed; on-chain verifiers check a single Schnorr signature against the committee public key.

Committees MUST satisfy:

- A threshold ratio of at least 2/3 of the committee size, consistent with classical BFT assumptions.
- A minimum committee size of 7 nodes; implementations producing attestations with smaller committees MUST mark the attestation as advisory rather than binding.
- The concrete committee size N and threshold T used to produce an attestation MUST be declared via the `committee_id` field and MUST be resolvable to a published Muon committee specification.

Larger committees (9–13 nodes) are RECOMMENDED for service categories with higher economic stakes. The minimum of 7 is a Sybil-resistance floor, not a recommendation.

### 11.3 Conformance Dimensions

**Latency.** Distribution of response times for `execute()` calls over a rolling window. Attestations report p50, p95, and p99 latencies — not single point estimates. The RECOMMENDED window is 30 days; implementations MAY choose shorter or longer windows, provided `window_start` and `window_end` are explicit in the attestation.

**Availability.** Fraction of polling attempts that returned a well-formed response within a hard timeout (e.g., 10× the declared SLA latency). Polling targets the real `execute()` endpoint with realistic inputs, not a dedicated `/health` endpoint, to prevent trivial gaming.

**Schema compliance.** Fraction of responses whose output validates against the provider's declared `output_schema`. This verifies *shape and type*, not *content correctness*. A provider returning `{"scores": [0.5]}` for every input is schema-compliant; whether 0.5 is the right answer is not addressed here (see §11.7).

### 11.4 Polling Protocol

To limit gaming, polling MUST follow these constraints:

- **Randomized timing.** Polls are scheduled with Poisson-distributed inter-arrival times. No fixed schedule a provider can prepare for.
- **Diverse origin.** Polls originate from rotating IP ranges and identities, not a fixed set of Muon-operator IPs.
- **Realistic inputs.** Polls draw inputs from a versioned, large input pool (see below). Pool entries are revealed only at polling time, not in advance.
- **Minimum committee size.** Implementations MUST NOT produce binding attestations with committees smaller than the minimum defined in §11.2.

#### Input Pool

The input pool is an append-only, public artifact maintained under version control. Contributions are open:

- Anyone MAY submit candidate inputs via a documented PR process.
- Each candidate MUST validate against the corresponding service category's `input_schema`.
- Acceptance criteria are mechanical (schema validation, deduplication, no PII) rather than discretionary.

At polling time, the Muon committee selects inputs from the pool using a verifiable random function (VRF). v1.2 implementations MUST use one of the following sources:

- Chainlink VRF v2 (recommended default for EVM deployments)
- A drand-style randomness beacon with on-chain anchoring
- A Muon-internal VRF construction, where the VRF output is signed by the same committee that produces the conformance attestation (use with caution: same-committee VRF creates a trust correlation between input selection and measurement)

Naive block-hash randomness MUST NOT be used.

Pool maintainership:

- **v1.2:** A single steward (the standard maintainer) merges PRs based on the mechanical acceptance criteria above. Discretionary rejection MUST be documented and appealable.
- **Future versions:** Per-category maintainers appointed via the governance process in §16.

This concentrates limited power in the steward: the steward controls what is *in* the pool, but the VRF prevents the steward from controlling *which* inputs are selected at any given poll.

#### Polling and Attestation Frequency

Polling frequency and attestation frequency are separately parameterized:

- Polling MUST be frequent enough to yield statistically meaningful latency distributions over the attestation window. Implementations SHOULD target a minimum sampling density of one poll per 15 minutes for any service category where p99 latency is reported.
- Attestation MAY aggregate multiple polls into a single signed artifact. Aggregation reduces on-chain anchoring cost without weakening the underlying statistical guarantees.

The relationship between the two is left to implementations, subject to the constraint that the attestation's metric fields MUST be derived from polls within the declared window.

#### Scope by Service Category

The polling protocol described in this section applies to service categories where:

1. inputs are stateless and inexpensive to construct,
2. provider responses are computable within bounded time, and
3. repeated polling does not create external side effects.

These conditions hold for the analysis categories (`sentiment_analysis`, `entity_extraction`, `text_classification`, `summarization`), `embedding_generation`, `translation`, and `document_parse`.

For categories where these conditions do not hold — including `code_execution`, `image_generation`, `web_fetch`, `database_query`, and `price_feed` — ARC-1 v1.2 does not specify a cryptographic conformance mechanism. Providers in these categories rely on:

- Self-declared SLAs in the `describe()` output, and
- Ex-post reputation derived from signed user reports (see §10).

Extending Section 11's mechanism to these categories is open work for future versions and may require category-specific protocols rather than a single generic approach.

### 11.5 Attestation Format

```json
{
  "provider_did": "did:arc1:0x1234abcd",
  "window_start": 1234560000,
  "window_end": 1237152000,
  "latency_p50_ms": 187,
  "latency_p95_ms": 412,
  "latency_p99_ms": 891,
  "availability_bps": 9987,
  "schema_compliance_bps": 10000,
  "poll_count": 2880,
  "committee_id": "muon-arc1-v1",
  "signature": "0x..."
}
```

All ratio metrics use basis points (`9987 = 99.87%`) to avoid floating-point ambiguity in on-chain comparison logic.

The example above represents a 30-day window with one poll per 15 minutes (2,880 polls), with the attestation signed and anchored once per window.

### 11.6 Cost Model

The economic model for compensating Muon node operators is outside the scope of ARC-1 v1.2. The standard specifies what an attestation looks like and what properties it MUST have; it does not specify what an attestation should cost, what node operators should be paid, or under what economic conditions a Muon committee can sustainably serve ARC-1 attestations.

This is a deliberate scope limitation, following precedents such as ERC-4337, which specifies the bundler role but not bundler economics. Different deployments may reach different economic equilibria, and some may not reach one at all.

v1.2 implementations MUST satisfy the following structural requirements:

- Providers MUST post a refundable deposit upon registration.
- The deposit MUST be slashable on cryptographic proof of provider fraud (e.g., signed responses contradicting attested behavior).
- Attestation fees MUST be paid to node operators in proportion to verified participation in the threshold signature. Non-signing nodes MUST NOT receive compensation.

Implementations are expected to find sustainable parameters through experimentation. The standard makes no claim that a sustainable equilibrium exists for any specific service category.

### 11.7 What ARC-1 v1.2 Does Not Verify

ARC-1 v1.2 deliberately does not attempt to verify that a provider returns *correct* or *high-quality* outputs. This is a known limitation, described here so that implementers and reviewers can understand what is and is not promised.

**The Volkswagen problem.** A provider that can identify when it is being polled — by IP, by request pattern, by input fingerprint, or by side channel — can return high-quality outputs to the polling committee while serving degraded outputs to real users. The mitigations in §11.4 raise the cost of this attack but do not eliminate it. Cryptographic guarantees against it require either (a) trusted hardware on the provider side (TEE-attested execution), or (b) zero-knowledge proofs of computation correctness — both outside the scope of ARC-1 v1.2.

**Goodhart's law on public benchmarks.** Any benchmark with publicly known inputs and expected outputs can be defeated by hard-coding the expected answers. A test case like `{input: "This is great!", expected: [0.5, 1.0]}` is satisfied trivially by a provider that returns `0.75` for that specific input and arbitrary outputs otherwise. This is not a hypothetical risk; it is a deterministic property of any public benchmark suite. ARC-1's input pool (§11.4) reduces this risk by withholding inputs until polling time, but a sufficiently determined adversary can still characterize the pool over time.

**Non-determinism of probabilistic outputs.** Many AI services return non-deterministic outputs (sampling-based generation). Consensus across multiple Muon nodes on "the correct answer" is ill-defined when two valid calls yield different outputs. Approaches such as semantic-equivalence checks or distributional tests exist in research but are not mature enough for v1.2 standardization.

**Ex-post reputation as partial substitute.** Where output quality cannot be verified ex-ante, ARC-1 routers (§10) MAY incorporate signed user reports of service outcomes into provider reputation. This shifts trust from cryptographic attestation to social/economic mechanisms, by design.

Future versions of ARC-1 may incorporate adversarial quality verification once the underlying primitives (TEE-attested execution, ZK-proven inference, or robust consensus on probabilistic outputs) are mature in production. v1.2 explicitly does not claim to.

---

## 12. Security

### 12.1 Three Attack Vectors

| Vector | Description | Solution |
| --- | --- | --- |
| Key theft | Attacker obtains private key | Spending limits + time-lock |
| Rogue agent | Sub-agent behaves maliciously | Budget management (timeout + refund flow) |
| Provider fraud | Provider takes payment, never delivers | Escrow + Muon verification |

### 12.2 Spending Limits & Time-lock

```
Even with a stolen key:
→ Damage limited by budget contract
→ Large transfers require 24h delay
→ Real owner can intervene in this window
```

### 12.3 Key Rotation

```
Agent detects: key compromised
→ calls rotate_key() on contract
→ new key is signed with old key
→ DID is updated to new key
→ budget and reputation are retained
```

### 12.4 Guardian Mechanism

```
Guardian (e.g. the human's wallet) can:
→ force key rotation
→ freeze budget
→ close all open channels
```

> Guardian is optional and must be defined at setup. The only human intervention point in the otherwise autonomous system.

---

## 13. Anti-Spam

### 13.1 The Problem

```
Provider X delivers poor quality
→ Reputation drops to 0.2
→ Router ignores Provider X
→ Provider X creates new wallet → fresh reputation 1.0
```

### 13.2 Four Protection Layers

**Layer 1 — Deposit:** Registration costs 10 USDC. New wallet = new deposit.

**Layer 2 — Progressive Deposit:**

```
First registration:                        10 USDC
Second registration (same infrastructure): 100 USDC
Third:                                    1000 USDC
```

**Layer 3 — Reputation Bootstrapping:**

```
New provider starts with reputation 0.5 (not 1.0)
→ Full reputation only after 30 days + 1000 real calls
```

**Layer 4 — Slashing + Blacklist:**

```
On proven fraud:
→ Deposit slashed
→ DID revoked
→ On-chain blacklist (permanent, DID-bound)
```

### 13.3 Overview

| Layer | Mechanism | Protects against |
| --- | --- | --- |
| Deposit | 10 USDC entry barrier | Trivial spam |
| Progressive deposit | Exponentially more expensive | Serial re-registrations |
| Reputation bootstrapping | Slow build-up | Reputation reset |
| Slashing + blacklist | DID-bound penalty | Active fraud |

---

## 14. Cross-Chain

### 14.1 The Problem

```
Agent has USDC on Base
Provider only accepts USDC on Solana
→ Direct payment not possible without bridging
```

### 14.2 Phased Rollout

| Phase | Approach | Status |
| --- | --- | --- |
| Phase 1 | Chain-matching via `supported_chains` | Actionable today |
| Phase 2 | Automatic bridging via Circle CCTP | Medium-term |
| Phase 3 | Muon as chain-agnostic settlement layer | Long-term |

### 14.3 Standard Definition

Every provider declares supported chains in `describe()`:

```json
"supported_chains": ["base", "solana", "polygon"]
```

---

## 15. Roadmap

### Phase 1 — Standard Definition (Month 1–3)

- Publish ARC-1 proposal as an open GitHub repository
- Define first service categories: Analysis, Compute, Data
- Gather community feedback (similar to the EIP process)
- Reference implementation in Rust + TypeScript

### Phase 2 — Muon Integration (Month 3–6)

- Develop Muon app for ARC-1 conformance checks
- Conformance suite for initial in-scope service categories
- Deploy on-chain attestation registry
- Certify first providers

### Phase 3 — Router Prototype (Month 6–12)

- Labor router as open-source reference implementation
- x402 + ARC-1 + Muon as an integrated stack
- Budget management smart contract as reference implementation
- Solana support via Muon's chain-agnostic architecture
- Developer documentation and SDKs

---

## 16. Open Questions & Honest Risks

### 16.1 Technical Risks

- Standardization is harder than protocol development — requires consensus
- Muon is still young (TGE April 2025) — dependency on an immature project
- x402 is EVM-only — Solana bridge requires separate work
- Payment channels with hierarchical delegation are complex — genuine research work remains here
- Section 11 covers only stateless-deterministic service categories — extending to compute-heavy or stateful categories is open work

### 16.2 Adoption Risks

- Chicken-and-egg: standard needs providers, providers need the standard
- Larger players (Anthropic, OpenAI) could set their own standards
- x402 hype could fade faster than standard adoption builds

### 16.3 What ARC-1 Is Not

- No token launch in the early phase — token follows adoption, not the other way around
- Not a Meridian competitor — ARC-1 is a layer above it
- Not a finished product — it is a proposal process

### 16.4 Governance Model

ARC-1 follows the most proven path of successful open standards: centralized stewardship in the early phase, gradual decentralization as adoption grows.

| Phase | Model | Precedent |
| --- | --- | --- |
| Phase 1 | Single steward, open GitHub proposals | TypeScript (Microsoft), GraphQL (Meta) |
| Phase 2 | Core team of 3–7 people | Linux Kernel, Python Software Foundation |
| Phase 3 | Neutral foundation, optional governance token | Kubernetes (Google → CNCF) |

**Three immutable principles:**

- ARC-1 remains royalty-free — no licensing fees for implementations
- Breaking changes require a supermajority — never unilaterally by one actor
- The standard belongs to the community — no single company can capture it

### 16.5 Compliance Boundaries

| Level | Keyword | Features |
| --- | --- | --- |
| Core | MUST | `describe()`, `quote()`, `execute()`, output schema, pricing unit, `model` field |
| Standard | SHOULD | Latency SLA, Muon attestation (where applicable), escrow flag, DID |
| Extended | MAY | Streaming payment, sub-agent delegation, penalty formula, cross-chain |

### 16.6 Versioning

```
ARC-1.MAJOR.MINOR.PATCH

PATCH → Documentation only
MINOR → Non-breaking changes, new optional features
MAJOR → Breaking changes, requires supermajority
```

**Three binding rules:**

1. **Compatibility window** — every MAJOR version supported in parallel with predecessor for 12 months
2. **Migration path** — every breaking change requires a documented migration path
3. **Deprecation before removal** — at least one MINOR version marked as deprecated before removal in a MAJOR version

---

## 17. Conclusion

The Agent Economy does not need another payment project. It needs the primitive that makes labor fungible — before routers, marketplaces, and DeFi-like protocols can emerge.

ARC-1 is that proposal. Muon is the verification layer for the subset of service categories where black-box conformance verification is technically defensible. x402 is the payment rail. The budget management framework is the solution for autonomous agent spending without a gatekeeper. The governance model secures the long-term neutrality of the standard.

> **Whoever sets the standard has more long-term influence than any single protocol built on top of it — just as Vitalik had more influence with ERC-20 than any individual DeFi protocol.**

---

## Changelog

| Version | Date | Contents |
| --- | --- | --- |
| v0.1 | May 2026 | Core concept: interface definition, quality metrics, payment interface, identity & attestation |
| v0.2 | May 2026 | Budget management: delegation, refund flow, payment channels |
| v0.3 | May 2026 | Error handling & dispute resolution: result states, Muon as verifier, penalty formula |
| v0.4 | May 2026 | Governance model: three-phase rollout, immutable principles, token strategy updated |
| v0.5 | May 2026 | Compliance boundaries (MUST/SHOULD/MAY) and versioning (MAJOR.MINOR.PATCH) |
| v0.6 | May 2026 | Interface specification, identity & DID, service categories, router specification |
| v0.7 | May 2026 | Benchmark suite, security, anti-spam, cross-chain |
| v1.0 | May 2026 | Executive summary updated, table of contents, changelog — first publication-ready version |
| v1.1 | May 2026 | `model` field in `describe()` promoted to MUST — core principle of model transparency |
| v1.2 | May 2026 | Section 11 renamed and rewritten as SLA Conformance Verification: honest scope limitation to externally observable properties (latency, availability, schema compliance); parametric committee sizing; append-only input pool with VRF-based selection; polling and attestation frequency separated; scope by service category made explicit; new §11.7 documenting Volkswagen problem, Goodhart's law on public benchmarks, and non-determinism limits. Sections 9 and 10 updated for consistency: per-service-type `conformance_mechanism` column in §9.1; `conformance_mechanism` field in §9.2; ranking formula per-category sourcing and `reputation_basis` field in §10. |

---

*ARC-1 — Agent Resource Contract Standard — v1.2 — Open Proposal*

*Contributions welcome via GitHub Issues and Pull Requests.*
