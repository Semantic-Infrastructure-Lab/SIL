# Trust Assertion Protocol (TAP)

> *The atomic unit of machine-readable trust*

## TL;DR

**What is TAP?** A Pantheon IR schema for expressing trust claims that both humans and agents can reason about.

**The core shape:**
```
Issuer → Claim → Subject + Context + Proof + Provenance
```

**Seven claim types:** `has-capability`, `has-credential`, `has-relationship`, `controls-key`, `has-history-with`, `passed-check`, `belongs-to`

**Why it matters:** Agent Ether needs to answer "Can Agent A do task T?" — TAP provides the typed, verifiable claims to answer that question.

---

## Architecture Integration

TAP is not a separate system — it's a cross-layer concern integrated into the Semantic OS:

| Semantic OS Layer | TAP Role |
|-------------------|----------|
| **Layer 6: Intelligence** | Agent Ether queries trust assertions for delegation decisions |
| **Layer 5: Intent** | Trust constraints and validation policies |
| **Layer 3: Composition** | TAP schema as Pantheon IR types |
| **Layer 2: Structures** | Trust assertions stored as GenesisGraph edges |

### How It Flows

1. **Agent Ether** needs to delegate task T to Agent A
2. **Query GenesisGraph** for trust assertions about Agent A
3. **Validate proofs** (ZK, signatures, etc.)
4. **Check context** (scope, domain, validity period)
5. **Decision** based on typed, verifiable claims — not reputation scores

### Relationship to Existing Components

| Component | Integration |
|-----------|-------------|
| **Pantheon IR** | TAP claim types are Pantheon semantic types |
| **GenesisGraph** | Trust assertions are typed edges with provenance |
| **Agent Ether** | Uses TAP for capability/safety verification |
| **Semantic Passport** | Bundle of TAP assertions for presentation |

---

## The Problem TAP Solves

Today's trust signals are:
- **Untyped** — skills, credentials, relationships all treated the same
- **Unverifiable** — résumés, bios, social graphs are claims without proof
- **Context-free** — a single "reputation score" replaces contextual evaluation
- **Siloed** — employment platforms, academic systems, code platforms don't interoperate
- **Opaque** — no proven provenance for trust claims

**Consequence:** Agents cannot reliably reason about who to trust. Humans cannot reliably evaluate agents. Institutions cannot reliably verify histories.

**TAP's solution:** Trust as typed, contextual, verifiable assertions with provenance.

---

## TAP vs Authorization — A Critical Distinction

**TAP proves trust and capability. Authorization proves permission.**

This is a fundamental architectural distinction in the Semantic OS:

| Concept | What It Proves | Semantic OS Primitive | Example |
|---------|---------------|---------------------|---------|
| **Trust Assertion (TAP)** | "Agent A *can* do task T" | TAP (this protocol) | `has-capability: deploy-production` |
| **Authorization** | "Agent A *may* do task T with constraints C" | AuthorizationGrant (Layer 1) | `permission: deploy-prod, budget: $1000, expires: 2025-12-31` |

### Why This Matters

**OS Architecture Perspective:**

Every multi-agent operating system needs two distinct permission models:
1. **Capability Model** — what the agent is technically able to do (skills, tools, access)
2. **Permission Model** — what the agent is granted authority to do (scope, constraints, expiration)

TAP provides (1). Authorization provides (2). Both are required.

**Agency Law Perspective:**

In legal agency theory (*Restatement Third of Agency*), an agent needs:
1. **Competence** — ability to perform the task
2. **Actual authority** — permission from principal with defined scope

TAP provides evidence of (1). Authorization provides (2).

**Practical Consequence:**

An agent with `has-capability: deploy-production` (TAP) can *technically* deploy, but without an **AuthorizationGrant** (permission from principal with scope/budget/expiration), that deployment would constitute **unauthorized agency** in both legal and OS security terms.

### Integration Pattern

Before Agent Ether delegates a task, it checks **both**:

```python
# Step 1: Check capability (TAP)
tap_assertion = query_tap(agent_id, "has-capability", "deploy-production")
if not tap_assertion:
    return Error("Agent lacks capability")

# Step 2: Check authorization (separate primitive)
auth_grant = query_authorization(agent_id, "deploy-production")
if not auth_grant or auth_grant.expired():
    return Error("Agent lacks permission or authorization expired")

# Step 3: Validate constraints
if exceeds_budget(task, auth_grant.budget):
    return Error("Task exceeds authorized budget")

# Both checks pass → delegate
delegate(agent_id, task)
```

### Failure Modes Without This Distinction

**If TAP = Authorization (conflated)**:
- Agent with capability auto-granted permission (security risk)
- No scope/budget/expiration enforcement (resource exhaustion)
- No audit trail of permission grants (compliance failure)
- Cannot revoke permission without removing capability (inflexible)

**With TAP + Authorization (separated)**:
- Agent needs both capability AND explicit permission grant
- Permission has scope, constraints, temporal bounds
- Full audit trail (GenesisGraph edges for both TAP and AuthorizationGrant)
- Revoke permission while preserving capability record

### See Also

- **AUTHORIZATION_PROTOCOL.md** — Complete specification of AuthorizationGrant primitive
- **HIERARCHICAL_AGENCY_FRAMEWORK.md** — Authority levels and delegation rules
- **SIL_GLOSSARY.md** — Definitions of Authorization, AuthorizationGrant, TAP

---

## Core Data Model

### Trust Assertion Schema

```json
{
  "$schema": "https://sil.org/schemas/tap/v1",
  "id": "tap:assertion:uuid",
  "version": "1.0.0",

  "issuer": "did:example:alice",
  "subject": "did:example:bob",

  "claim": {
    "type": "has-capability",
    "value": "distributed-systems",
    "level": "expert",
    "evidence": ["commit:abcd1234", "paper:xyz"]
  },

  "context": {
    "scope": "job-application",
    "domain": "software-engineering",
    "valid_from": "2023-01-01T00:00:00Z",
    "valid_to": "2026-01-01T00:00:00Z",
    "constraints": []
  },

  "proof": {
    "type": "zk-snark",
    "statement": "Subject has completed >50 commits to repo X",
    "verifier": "https://verify.sil.org/zk",
    "data": "..."
  },

  "provenance": {
    "graph_node": "gg:node:12345",
    "timestamp": "2025-02-15T12:00:00Z",
    "source": "github",
    "chain": ["gg:node:12344", "gg:node:12343"]
  }
}
```

### Required Fields

| Field | Type | Description |
|-------|------|-------------|
| `id` | URI | Unique identifier for this assertion |
| `version` | SemVer | TAP schema version |
| `issuer` | DID | Who is making this claim |
| `subject` | DID | Who/what the claim is about |
| `claim` | ClaimObject | The trust claim itself |

### Optional Fields

| Field | Type | Description |
|-------|------|-------------|
| `context` | ContextObject | When/where this claim applies |
| `proof` | ProofObject | Cryptographic verification |
| `provenance` | ProvenanceObject | GenesisGraph lineage |

---

## Claim Types

TAP defines a minimal, universal ontology for trust claims. This vocabulary is intentionally small — it should cover all human and agent trust patterns without becoming unwieldy.

### `has-capability`

Asserts that the subject possesses a skill or competency.

```json
{
  "type": "has-capability",
  "value": "distributed-systems",
  "level": "expert",
  "evidence": ["commit:abc123", "project:xyz"],
  "confidence": 0.95
}
```

**Levels:** `novice`, `intermediate`, `advanced`, `expert`, `authoritative`

**Use cases:**
- Technical skills for hiring
- Agent capability advertisement
- Tool proficiency claims

### `has-credential`

Asserts that the subject holds a formal credential.

```json
{
  "type": "has-credential",
  "credential": "PhD",
  "field": "Computer Science",
  "issuer_name": "Stanford University",
  "issued": "2020-06-15",
  "expires": null
}
```

**Use cases:**
- Academic degrees
- Professional licenses
- Certifications (AWS, PMP, etc.)
- Agent safety certifications

### `has-relationship`

Asserts a relationship between the subject and another entity.

```json
{
  "type": "has-relationship",
  "relationship": "mentored-by",
  "target": "did:example:mentor",
  "duration": "P2Y",
  "context": "distributed-systems-research"
}
```

**Relationship types:**
- `mentored-by` / `mentor-of`
- `collaborated-with`
- `employed-by` / `employer-of`
- `friend-of`
- `supervised-by` / `supervisor-of`
- `co-authored-with`

### `controls-key`

Asserts cryptographic key ownership or control.

```json
{
  "type": "controls-key",
  "key_id": "did:key:z6MkhaXgBZDvotDkL5257faiztiGiC2QtKLGpbnnEGta2doK",
  "key_type": "Ed25519",
  "purpose": ["authentication", "assertion"]
}
```

**Use cases:**
- Identity binding
- Signature authority
- Agent identity verification

### `has-history-with`

Asserts a track record of past interactions.

```json
{
  "type": "has-history-with",
  "target": "did:example:client",
  "interaction_type": "project-completion",
  "count": 5,
  "success_rate": 1.0,
  "timespan": "P3Y"
}
```

**Use cases:**
- Work history verification
- Agent performance records
- Collaboration track records

### `passed-check`

Asserts successful completion of a verification process.

```json
{
  "type": "passed-check",
  "check_type": "safety-evaluation",
  "standard": "sil-agent-safety-v1",
  "result": "pass",
  "score": 0.97,
  "evaluated_at": "2025-01-15T10:00:00Z",
  "valid_until": "2026-01-15T10:00:00Z"
}
```

**Check types:**
- Background checks
- Safety evaluations
- Compliance audits
- Skill assessments
- Security clearances

### `belongs-to`

Asserts membership in an organization or group.

```json
{
  "type": "belongs-to",
  "organization": "did:web:ieee.org",
  "role": "member",
  "since": "2018-03-01",
  "status": "active"
}
```

**Use cases:**
- Professional memberships
- Organizational affiliations
- Agent registry membership

---

## Context Object

Context constrains when and where a trust assertion applies.

```json
{
  "context": {
    "scope": "job-application",
    "domain": "software-engineering",
    "valid_from": "2023-01-01T00:00:00Z",
    "valid_to": "2026-01-01T00:00:00Z",
    "geographic": "US",
    "constraints": [
      { "type": "purpose-limitation", "value": "hiring-only" },
      { "type": "delegation-depth", "value": 0 }
    ]
  }
}
```

### Context Fields

| Field | Type | Description |
|-------|------|-------------|
| `scope` | string | Purpose of this assertion |
| `domain` | string | Knowledge/industry domain |
| `valid_from` | ISO8601 | Start of validity period |
| `valid_to` | ISO8601 | End of validity period (null = indefinite) |
| `geographic` | string | Geographic applicability |
| `constraints` | array | Additional usage constraints |

**Key principle:** Trust is contextual. A capability claim for "hiring" may not apply to "system access." Context makes this explicit.

---

## Proof Object

Proofs provide cryptographic verification of claims.

### Proof Types

#### `signature`
Simple cryptographic signature by the issuer.

```json
{
  "type": "signature",
  "algorithm": "Ed25519",
  "value": "base64-encoded-signature",
  "verificationMethod": "did:example:alice#keys-1"
}
```

#### `zk-snark` / `zk-stark`
Zero-knowledge proof that a statement is true without revealing underlying data.

```json
{
  "type": "zk-snark",
  "statement": "Subject has >50 commits to public repositories",
  "circuit": "tap:circuit:commit-count-v1",
  "proof": "base64-encoded-proof",
  "public_inputs": ["50"],
  "verifier": "https://verify.sil.org/zk/commit-count"
}
```

#### `sd-jwt`
Selective Disclosure JWT — reveal only chosen claims.

```json
{
  "type": "sd-jwt",
  "jwt": "eyJ...",
  "disclosures": ["employment_years", "role"],
  "holder_binding": "did:example:bob"
}
```

#### `merkle-inclusion`
Prove membership in a set without revealing the set.

```json
{
  "type": "merkle-inclusion",
  "root": "sha256:abc123...",
  "path": ["left:def456", "right:ghi789"],
  "leaf": "sha256:jkl012..."
}
```

#### `bbs-plus`
BBS+ signature with unlinkable selective disclosure.

```json
{
  "type": "bbs-plus",
  "signature": "base64-encoded",
  "disclosed_indices": [0, 2, 5],
  "proof": "base64-encoded-proof"
}
```

---

## Provenance Object

Links the trust assertion to GenesisGraph for full lineage tracking.

```json
{
  "provenance": {
    "graph_node": "gg:node:12345",
    "timestamp": "2025-02-15T12:00:00Z",
    "source": "github",
    "source_id": "commit:abc123",
    "chain": ["gg:node:12344", "gg:node:12343"],
    "transformations": [
      {
        "operation": "extract-commit-metadata",
        "tool": "github-api-v4",
        "timestamp": "2025-02-15T11:59:00Z"
      }
    ]
  }
}
```

**Integration with GenesisGraph:**
- Trust assertions become edges in the semantic graph
- Full provenance chain is preserved
- Supports Level A/B/C disclosure (full, partial, sealed)

---

## Semantic Passport

A **Semantic Passport** bundles relevant trust assertions for a specific purpose.

```json
{
  "$schema": "https://sil.org/schemas/tap/passport/v1",
  "id": "tap:passport:uuid",
  "subject": "did:example:scott",
  "purpose": "senior-ai-engineer-application",
  "created": "2025-06-08T14:00:00Z",

  "assertions": [
    { "$ref": "tap:assertion:capability-distributed-systems" },
    { "$ref": "tap:assertion:credential-microsoft-employment" },
    { "$ref": "tap:assertion:relationship-mentor-connor" },
    { "$ref": "tap:assertion:history-oss-contributions" }
  ],

  "presentation_proof": {
    "type": "signature",
    "algorithm": "Ed25519",
    "value": "...",
    "verificationMethod": "did:example:scott#keys-1"
  }
}
```

**Use cases:**
- Job applications
- Contract negotiations
- Agent capability presentation
- Access requests

---

## Verification Flow

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   Subject   │────▶│  Verifier   │────▶│  Decision   │
│ (presents   │     │ (checks)    │     │ (grants or  │
│  passport)  │     │             │     │  denies)    │
└─────────────┘     └─────────────┘     └─────────────┘
                           │
                           ▼
              ┌────────────────────────┐
              │     Verification       │
              │  ┌──────────────────┐  │
              │  │ 1. Identity (DID) │  │
              │  │ 2. Signatures     │  │
              │  │ 3. ZK Proofs      │  │
              │  │ 4. Context Match  │  │
              │  │ 5. Provenance     │  │
              │  │ 6. Expiration     │  │
              │  └──────────────────┘  │
              └────────────────────────┘
```

### Verification Steps

1. **Identity Resolution** — Resolve DIDs to DID Documents
2. **Signature Verification** — Verify issuer signatures
3. **Proof Validation** — Validate ZK proofs, SD-JWTs, etc.
4. **Context Matching** — Check scope, domain, validity period
5. **Provenance Check** — Verify GenesisGraph lineage (optional)
6. **Policy Evaluation** — Apply verifier-specific policies

---

## Agent-Specific Extensions

TAP includes extensions for AI agent trust:

### Agent Capability Assertion

```json
{
  "type": "has-capability",
  "value": "code-execution",
  "agent_specific": {
    "execution_environment": "sandboxed-docker",
    "resource_limits": {
      "memory": "4GB",
      "cpu": "2 cores",
      "network": "restricted"
    },
    "safety_constraints": ["no-external-api", "read-only-fs"]
  }
}
```

### Agent Safety Attestation

```json
{
  "type": "passed-check",
  "check_type": "agent-safety-evaluation",
  "standard": "sil-agent-safety-v1",
  "agent_specific": {
    "model_family": "claude",
    "evaluation_dataset": "sil-safety-benchmark-2025",
    "behavioral_tests": {
      "instruction_following": 0.98,
      "harm_refusal": 0.99,
      "capability_boundaries": 0.97
    }
  }
}
```

### Agent Provenance Chain

```json
{
  "type": "has-history-with",
  "interaction_type": "task-execution",
  "agent_specific": {
    "task_type": "code-review",
    "total_tasks": 1247,
    "success_rate": 0.994,
    "average_latency_ms": 3200,
    "human_override_rate": 0.02,
    "provenance_root": "gg:node:agent-history-root"
  }
}
```

---

## Examples

### Human Hiring

A Semantic Passport for a senior AI systems role:

```yaml
trust_assertions:
  - type: has-relationship
    claim: "mentored-by"
    subject: "did:key:scott"
    issuer: "did:key:connor-cunningham"
    proof: { type: "signature", value: "..." }

  - type: has-capability
    claim: "distributed-systems"
    level: "expert"
    evidence: ["commit:abc123", "paper:xyz"]
    proof: { type: "zk-snark", statement: ">50 commits to distributed systems repos" }

  - type: has-credential
    claim: "employment"
    issuer: "did:web:microsoft.com"
    context: { role: "Senior Engineer", years: 5 }
    proof: { type: "sd-jwt", disclosed: ["role", "years"] }
```

Everything verifiable. No résumé. No bluffing. No vibes.

### Agent Authorization

An autonomous agent requesting access to a sensitive workflow:

```yaml
trust_assertions:
  - type: has-capability
    claim: "code-execution"
    level: "sandboxed"
    proof: { type: "signature", issuer: "did:web:anthropic.com" }

  - type: passed-check
    claim: "safety-evaluation"
    standard: "sil-agent-safety-v1"
    proof: { type: "zk-range", statement: "safety_score > 0.95" }

  - type: has-history-with
    claim: "successful-tasks"
    count: 147
    context: { domain: "code-review", failure_rate: "<1%" }
    proof: { type: "merkle-inclusion", root: "sha256:..." }
```

Trust for safe delegation — typed, verified, contextual.

---

## JSON-LD Context

TAP assertions use JSON-LD for semantic interoperability:

```json
{
  "@context": [
    "https://www.w3.org/2018/credentials/v1",
    "https://sil.org/contexts/tap/v1",
    {
      "tap": "https://sil.org/tap#",
      "gg": "https://sil.org/genesisgraph#",
      "has-capability": "tap:HasCapability",
      "has-credential": "tap:HasCredential",
      "has-relationship": "tap:HasRelationship"
    }
  ]
}
```

---

## Relationship to Standards

| Standard | TAP Relationship |
|----------|------------------|
| W3C Verifiable Credentials | TAP assertions are VCs with SIL-specific claim types |
| W3C DIDs | TAP uses DIDs for issuer/subject identification |
| IETF SD-JWT | TAP supports SD-JWT as a proof type |
| BBS+ Signatures | TAP supports BBS+ for unlinkable disclosure |
| Pantheon IR | TAP claim types are Pantheon semantic types |
| GenesisGraph | TAP provenance integrates with GG nodes |

---

## Security Considerations

1. **Issuer Authentication** — Always verify issuer DID controls the signing key
2. **Replay Prevention** — Use unique assertion IDs and timestamps
3. **Context Enforcement** — Don't accept assertions outside their declared scope
4. **Provenance Verification** — For high-stakes decisions, verify full GenesisGraph chain
5. **ZK Soundness** — Use well-audited ZK circuits; verify trusted setup where applicable
6. **Key Rotation** — Support DID key rotation and revocation

---

## Future Extensions

- **Delegation chains** — A trusts B, B vouches for C
- **Threshold assertions** — N-of-M issuers must agree
- **Temporal reasoning** — Trust decay over time
- **Negative assertions** — Explicit distrust claims
- **Capability composition** — Combine capabilities into complex profiles

---

## Related Documentation

- **[GenesisGraph](../innovations/GENESISGRAPH.md)** — Provenance substrate and trust storage
- **[Agent Ether](../innovations/AGENT_ETHER.md)** — Multi-agent coordination using TAP
- **[SIL Glossary](./SIL_GLOSSARY.md)** — Term definitions
- **[Semantic OS Architecture](./SIL_SEMANTIC_OS_ARCHITECTURE.md)** — Layer definitions

---

**Version:** 1.0.0-draft
**Status:** Specification draft
**License:** Apache 2.0
