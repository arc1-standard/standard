# Contributing to ARC-1

ARC-1 is an open standard — not a product. Contributions shape a shared primitive for the Agent Economy. This document explains how to get involved.

---

## Types of Contributions

There are four ways to contribute, each with a different process:

| Type | Examples | Process |
|---|---|---|
| **Standard Proposals** | New service types, new fields in `describe()` | ARC-1 Proposal (ARP) |
| **Spec Corrections** | Typos, ambiguities, broken references | Pull Request directly |
| **Implementations** | Router, SDK, provider example | Pull Request + discussion |
| **Discussion** | Architecture feedback, use cases | GitHub Issue |

---

## ARC-1 Proposals (ARPs)

A proposal is required for any change that affects the standard itself — new service categories, new required fields, changes to existing interfaces, or changes to the compliance framework.

### How to Submit a Proposal

1. **Open an Issue** with the title prefix `[ARP]` — e.g. `[ARP] Add code_execution service category`
2. Describe the following in the Issue body:
   - **Problem** — what gap does this fill?
   - **Proposed change** — concrete, specific, no vague directions
   - **Backwards compatibility** — is this MINOR (additive) or MAJOR (breaking)?
   - **Example** — at minimum a JSON example of the new/changed field
3. Allow **7 days** for community discussion before a decision is made
4. If approved, open a Pull Request that implements the spec change

### What Makes a Good Proposal

- **Specific** — "add `confidence` as an optional output field in all Analysis categories" not "improve output quality"
- **Backwards-compatible** — new fields should be `MAY` unless there is a compelling reason for `MUST`
- **Precedented** — reference existing service categories or real provider behavior where possible

### What Gets Rejected

- Token proposals or governance changes in the early phase
- Breaking changes without a documented migration path
- Proposals that make ARC-1 specific to one chain, one model provider, or one payment rail

---

## Pull Requests

For spec corrections and implementations, open a Pull Request directly.

**Branch naming:**
- `fix/` — corrections to existing spec text
- `feat/` — new content after an ARP was approved
- `impl/` — implementations (router, SDK, provider examples)

**PR checklist:**
- [ ] The change is backwards-compatible, or references an approved ARP
- [ ] JSON examples are valid and consistent with the spec
- [ ] No new `MUST` requirements without ARP approval

---

## Versioning

ARC-1 follows `MAJOR.MINOR.PATCH`:

- `PATCH` — documentation and wording only, no semantic change
- `MINOR` — additive, non-breaking changes (new optional fields, new service types)
- `MAJOR` — breaking changes, requires supermajority of core contributors

When in doubt, open an Issue before writing code.

---

## Code of Conduct

ARC-1 is protocol-focused. Keep discussions technical and constructive. Proposals live or die by their merits, not by who submits them.

---

## Questions

Open a GitHub Issue with the `question` label. No question is too basic — the goal is a standard that any developer can implement without guesswork.
