# ARC-1 — Agent Resource Contract Standard

**An open standard for the Agent Economy**

Version 1.0 — May 2026 | [Changelog](#changelog) | [Implementation Guide](ARC-1_ImplementationGuide.md)

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
11. [Benchmark Suite](#11-benchmark-suite)
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

ARC-1 v1.0 defines seven core areas:

| Area | What it solves |
|---|---|
| Interface Specification | `describe()`, `quote()`, `execute()` as mandatory core functions |
| Quality Metrics | Standardized SLA declaration and benchmark suite via Muon |
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
|---|---|---|
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
- **Accuracy Benchmark** — standardized test suite per service category
- **Uptime Requirement** — minimum availability (e.g. 99.5%)
- **Confidence Score** — mandatory output field for verifiable quality

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

```
┌──────────────────────────────┐
│     INTENT LAYER             │  ← Agent formulates goals
├──────────────────────────────┤
│     ROUTING LAYER            │  ← Automatic provider selection
├──────────────────────────────┤
│     ARC-1 STANDARD           │  ← Fungibility / The primitive
├──────────────────────────────┤
│  MUON (Verification Layer)   │  ← Trustless quality checks
├──────────────────────────────┤
│  x402 / MERIDIAN (Payment)   │  ← Execution rail
└──────────────────────────────┘
```

### 4.1 Role of Muon

- Muon nodes run benchmark requests against ARC-1 endpoints
- Nodes sign compliance proofs via TSS (Threshold Signature Scheme)
- Signatures are on-chain verifiable — decentralized certification without a central authority
- Historical performance data becomes available as a routing signal

### 4.2 Role of x402 / Meridian

x402 remains what it is: an elegant convenience layer for payment triggering in HTTP requests. In the ARC-1 stack, x402 is the execution layer — not the foundation.

> **Important:** ARC-1 does not depend on Meridian. Any x402-compatible payment system can serve as the rail.

---

## 5. Why This Is Genuinely New

### 5.1 Comparison with Existing Solutions

| Project | What it solves | What is missing |
|---|---|---|
| Meridian / x402 | Payment UX for agents | Standard, routing, reputation |
| Muon | Trustless off-chain validation | No payment, no service standard |
| Chainlink | Price feed oracle | No agent labor standard |
| Virtuals Protocol | Agent tokenization | No interoperable payment interface |
| **ARC-1 (Proposal)** | **Fungibility of agent services** | **← Fills this gap** |

### 5.2 The ERC-20 Analogy

| ERC-20 World | ARC-1 World |
|---|---|
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
|---|---|---|
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
  "arc1_version": "1.0.0",
  "provider_did": "did:arc1:0x1234abcd",
  "input_schema": { "texts": ["string"] },
  "output_schema": { "scores": ["float -1..1"] },
  "pricing_unit": "per_1000_tokens",
  "accepted_tokens": ["USDC"],
  "supported_chains": ["base", "solana"],
  "sla": { "latency_ms": 500, "uptime": 99.5 },
  "compliance_level": "standard"
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
|---|---|---|---|
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

| Category | Service Types |
|---|---|
| Analysis | `sentiment_analysis`, `entity_extraction`, `text_classification`, `summarization` |
| Compute | `code_execution`, `image_generation`, `embedding_generation`, `translation` |
| Data | `web_fetch`, `database_query`, `price_feed`, `document_parse` |

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
  "benchmark_suite": "arc1-sentiment-v1",
  "pricing_unit": "per_1000_tokens"
}
```

> New categories are MINOR changes — no breaking change, no supermajority required.

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
Score = (Price score     × 0.4)
      + (Latency score   × 0.3)
      + (Reputation score × 0.3)
```

- **Price score** — cheapest provider gets 1.0, most expensive gets 0.0
- **Latency score** — from Muon attestation, historical SLA adherence
- **Reputation score** — ratio of SUCCESS to FAILED/PARTIAL over the last 30 days

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
  "arc1_version": "1.0.0"
}
```

### 10.5 Router Rollout

| Phase | Approach | Status |
|---|---|---|
| Phase 1 | Open-source reference implementation | Actionable today |
| Phase 2 | Muon as decentralized router | Depends on Muon maturity |
| Phase 3 | Many independent router implementations | Result of Phase 1+2 |

---

## 11. Benchmark Suite

### 11.1 Muon as Decentralized Tester

```
Muon node → Provider: execute() with standard input
Muon node measures: latency, schema compliance, availability
Muon nodes compare results via consensus
→ Sign attestation: "Provider X met SLA"
→ On-chain verifiable
```

### 11.2 Benchmark Suite Structure

```json
{
  "suite": "arc1-sentiment-v1",
  "test_cases": [
    {
      "input": { "texts": ["This is great!"] },
      "expected_range": { "scores": [0.5, 1.0] },
      "max_latency_ms": 500
    }
  ],
  "frequency": "every_6_hours",
  "min_nodes": 3
}
```

### 11.3 Measurement Dimensions

- **Latency** — median over 30 days
- **Uptime** — availability rate over 30 days
- **Accuracy** — output within declared range

### 11.4 Cost Model

```
Provider registers:
→ pays 10 USDC deposit
→ deposit covers ~6 months of benchmarking
→ Muon nodes are paid proportionally from deposit
→ after 6 months: top up or deregistration
```

---

## 12. Security

### 12.1 Three Attack Vectors

| Vector | Description | Solution |
|---|---|---|
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
|---|---|---|
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
|---|---|---|
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

- Develop Muon app for ARC-1 compliance checks
- Benchmark suite for initial service categories
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
|---|---|---|
| Phase 1 | Single steward, open GitHub proposals | TypeScript (Microsoft), GraphQL (Meta) |
| Phase 2 | Core team of 3–7 people | Linux Kernel, Python Software Foundation |
| Phase 3 | Neutral foundation, optional governance token | Kubernetes (Google → CNCF) |

**Three immutable principles:**
- ARC-1 remains royalty-free — no licensing fees for implementations
- Breaking changes require a supermajority — never unilaterally by one actor
- The standard belongs to the community — no single company can capture it

### 16.5 Compliance Boundaries

| Level | Keyword | Features |
|---|---|---|
| Core | MUST | `describe()`, `quote()`, `execute()`, output schema, pricing unit |
| Standard | SHOULD | Latency SLA, Muon attestation, escrow flag, DID |
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

ARC-1 is that proposal. Muon is the verification layer. x402 is the payment rail. The budget management framework is the solution for autonomous agent spending without a gatekeeper. The governance model secures the long-term neutrality of the standard.

> **Whoever sets the standard has more long-term influence than any single protocol built on top of it — just as Vitalik had more influence with ERC-20 than any individual DeFi protocol.**

---

## Changelog

| Version | Date | Contents |
|---|---|---|
| v0.1 | May 2026 | Core concept: interface definition, quality metrics, payment interface, identity & attestation |
| v0.2 | May 2026 | Budget management: delegation, refund flow, payment channels |
| v0.3 | May 2026 | Error handling & dispute resolution: result states, Muon as verifier, penalty formula |
| v0.4 | May 2026 | Governance model: three-phase rollout, immutable principles, token strategy updated |
| v0.5 | May 2026 | Compliance boundaries (MUST/SHOULD/MAY) and versioning (MAJOR.MINOR.PATCH) |
| v0.6 | May 2026 | Interface specification, identity & DID, service categories, router specification |
| v0.7 | May 2026 | Benchmark suite, security, anti-spam, cross-chain |
| v1.0 | May 2026 | Executive summary updated, table of contents, changelog — first publication-ready version |

---

*ARC-1 — Agent Resource Contract Standard — v1.0 — Open Proposal*

*Contributions welcome via GitHub Issues and Pull Requests.*
