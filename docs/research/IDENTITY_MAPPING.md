# Identity Mapping - Universal Cross-Domain Identity Resolution

**Research Question:** How do we resolve identities across heterogeneous semantic domains in a universal, verifiable, and composable way?

---

## 🎯 The Problem

### Identity Fragmentation

Every semantic domain maintains its own identifier namespace:

```
Person: "Alice"
  contacts://        → alice@example.com (email)
  slack://          → U1234567 (user_id)
  github://         → alice-dev (username)
  mysql://users     → 42 (primary_key)
  pantheon://       → person:alice:canonical (semantic_id)
```

**Challenges:**
1. **No universal resolver** - Each system uses its own IDs
2. **Manual translation** - Converting email → user_id requires lookup tables
3. **Fragile integration** - Cross-system queries break when IDs change
4. **Lost semantics** - Systems don't know IDs refer to same entity

**Example:** Agent Ether wants to notify "alice@example.com" via Slack:
```python
# Current: Manual lookup required
email = "alice@example.com"
user = db.query("SELECT slack_id FROM users WHERE email = ?", email)
slack.send(user.slack_id, "Task complete")

# Desired: Universal resolution
email = "alice@example.com"
slack_id = mapper.resolve(email, target="slack")
slack.send(slack_id, "Task complete")
```

---

## 🏗️ Architectural Position

### Layer Assignment

**Primary Home: Layer 1 (Universal Semantic IR - Pantheon)**

**Rationale:**
1. **Identity is semantic** - Recognizing that different signifiers refer to the same referent is a core semantic problem
2. **Foundational primitive** - Higher layers (composition, orchestration) depend on identity resolution
3. **Domain-agnostic** - Works across all SIL projects (morphogen, tiacad, reveal, etc.)
4. **Type system** - Identities have types (email, username, uuid) - structural semantics

**Also: Cross-Cutting Concern (like Provenance)**

**Rationale:**
1. **Every layer has identities** - From Layer 0 (file descriptors) to Layer 7 (user emails)
2. **Universal access** - All layers need to resolve identities
3. **Non-intrusive** - Doesn't belong to any single layer exclusively

**Mental Model:**
```
┌──────────────────────────────────────────┐
│  All Layers (7-0) consume mapper API     │
└─────────────┬────────────────────────────┘
              │
      ┌───────▼─────────┐
      │ Mapper API      │  ← Cross-cutting service
      │ (owl:sameAs)    │
      └───────┬─────────┘
              │
      ┌───────▼─────────┐
      │ Pantheon        │  ← Primary storage
      │ (Layer 1)       │     (semantic nodes + identities)
      └─────────────────┘
```

---

## 📐 Theoretical Foundation

### Semantic Web Precedent

**RDF/OWL `owl:sameAs` predicate:**
```turtle
<http://example.com/person/alice> owl:sameAs <mailto:alice@example.com> .
<mailto:alice@example.com> owl:sameAs <slack://U1234567> .
```

**Properties:**
- **Transitive:** A=B, B=C → A=C
- **Symmetric:** A=B → B=A
- **Reflexive:** A=A

**Limitation:** Semantic Web focused on *URIs*. We need resolution across *arbitrary domain identifiers*.

### Type Theory

Identity mapping introduces a **universal equivalence relation** across domain-specific type systems:

```
Domain_A :: Type_A → Entity
Domain_B :: Type_B → Entity

mapper :: (Domain_A, Type_A, ID_A) → (Domain_B, Type_B, ID_B)

Property: ∀ domains A,B,C: mapper(A→B) ∘ mapper(B→C) = mapper(A→C)
```

**This is a functor between domain categories.**

### Information Theory

Identity resolution is **semantic compression**:
- Store canonical entity once (Pantheon node)
- Maintain mapping edges (low cost)
- Resolve on demand (avoid duplication)

**Bit savings:**
```
Without mapper:
  N systems × M entities × avg_record_size
  = 10 systems × 10K entities × 200 bytes = 20MB

With mapper:
  M entities × avg_record_size + N×M mappings × 16 bytes
  = 10K × 200 bytes + 100K × 16 bytes = 3.6MB

Compression: 5.5x
```

---

## 🔬 Research Agenda

### Phase 1: Formal Specification (Months 1-2)

**Deliverables:**
1. Formal identity type system
2. Resolution algorithm specification
3. Consistency invariants
4. Security model (who can assert identity equivalence?)

**Key Questions:**
- How to handle ambiguity (one identifier → multiple entities)?
- Temporal semantics (identities change over time)?
- Trust model (who is authoritative for which domains)?

### Phase 2: Pantheon Integration (Months 3-6)

**Deliverables:**
1. Pantheon node schema extension (identities field)
2. Resolution API implementation
3. Query language for identity relationships
4. Provenance integration (GenesisGraph attestations)

**Technical Design:**
```yaml
# Pantheon node with identities
node:
  id: person:alice:canonical
  type: Person
  properties:
    name: "Alice Developer"

  identities:
    - domain: contacts
      type: email
      identifier: alice@example.com
      authority: user-declared
      valid_from: 2020-01-01

    - domain: slack
      type: user_id
      identifier: U1234567
      display: "@alice"
      authority: api-verified
      verified_at: 2025-12-01
```

### Phase 3: Interface Layer (Months 6-9)

**Deliverables:**
1. Reveal URI adapter (`reveal map://contacts/email → slack`)
2. CLI tool (`tia map resolve ...`)
3. Agent Ether integration (agents use mapper for routing)
4. Documentation + examples

### Phase 4: Advanced Features (Months 9-12)

**Deliverables:**
1. Auto-discovery (infer mappings from data)
2. Fuzzy matching (handle typos, variations)
3. Federated registries (distributed identity resolution)
4. Machine learning (suggest mappings)

---

## 💡 Novel Contributions

### 1. Domain-Agnostic Resolution

**Innovation:** Works across *any* identifier scheme, not just URIs/URLs

**Comparison:**
- DNS: domain names → IP addresses (single domain)
- OAuth: service tokens → user identity (authentication-specific)
- ORCID: researcher IDs (academia-specific)
- **Mapper:** arbitrary_domain_A → arbitrary_domain_B (universal)

### 2. Composable with Pantheon IR

**Innovation:** Identity mapping is *part of* the semantic graph, not external

**Benefits:**
- Queries can traverse identity edges
- Provenance applies to mappings (who asserted this equivalence?)
- Same query language for entities and identities

### 3. Progressive Disclosure via Reveal

**Innovation:** Identity resolution has same UX as resource exploration

```bash
# Structure first (see all identities)
reveal map://contacts/alice@example.com

# Drill down (specific mapping)
reveal map://contacts/alice@example.com --to slack

# Extract (machine-readable)
reveal map://contacts/alice@example.com --to slack --format json
```

---

## 🎯 Success Criteria

### Theoretical

1. **Formally verified** identity resolution algorithm
2. **Proven consistency** under concurrent updates
3. **Bounded resolution time** O(log N) for N identities
4. **Compositional semantics** (mappings compose algebraically)

### Practical

1. **Adoption** across 3+ SIL projects (Pantheon, Reveal, Agent Ether)
2. **Performance** <10ms resolution for 99th percentile
3. **Scale** 1M+ entities, 10M+ identity mappings
4. **Usability** Non-technical users can add mappings

---

## 🔗 Integration with SIL Ecosystem

### Layer 0: Semantic Memory
**Use:** Store mapping registry efficiently (SQLite or Pantheon native)

### Layer 1: Pantheon (Primary Home)
**Use:** Canonical semantic nodes with identity aliases

### Layer 2: Domain Modules
**Use:** Each module (morphogen, tiacad) can resolve identities in its domain

### Layer 3: Agent Ether
**Use:** Agents resolve identities for message routing, tool invocation

### Layer 5: Reveal
**Use:** User interface for exploring identity mappings via `map://` URI

### Cross-Cutting: GenesisGraph
**Use:** Provenance for identity assertions (who claimed A=B?)

---

## 📚 Related Research

**Semantic Web:**
- RDF `owl:sameAs` predicate
- FOAF (Friend of a Friend) project
- Linked Data principles

**Identity Systems:**
- W3C DID (Decentralized Identifiers)
- ORCID (researcher identifiers)
- OAuth/OIDC (authentication identity)

**Database Theory:**
- Foreign key relationships
- Entity resolution / record linkage
- Data integration

**Type Theory:**
- Functors between categories
- Universal constructions
- Type equivalence

**Key Difference:** Existing systems are domain-specific or authentication-focused. Identity mapping is **universal and semantic**.

---

## 🚀 Next Steps

1. **Formalize specification** (this document → formal paper)
2. **Prototype in TIA** (validate core concepts)
3. **Design Pantheon integration** (node schema + API)
4. **Build Reveal adapter** (user interface)
5. **Publish research** (arXiv, SIL website)

---

## 📖 References

**Internal:**
- [SIL Manifesto](../canonical/SIL_MANIFESTO.md) - Why explicit semantics matter
- [Unified Architecture Guide](../architecture/UNIFIED_ARCHITECTURE_GUIDE.md) - Layer structure
- [Pantheon](../innovations/PANTHEON.md) - Universal Semantic IR

**External:**
- Berners-Lee, T. "Linked Data" (2006)
- W3C OWL Web Ontology Language
- Elmagarmid, A. et al. "Duplicate Record Detection" (2007)

---

**Document Status:** Proposed Research Concept
**Last Updated:** 2025-12-02
**Originated:** Semantic glue exploration

---

## Appendix: Example Use Cases

### Use Case 1: Cross-System Queries
```bash
# Find all GitHub PRs by user with email alice@example.com
email="alice@example.com"
github_user=$(mapper resolve contacts://$email --to github)
gh pr list --author $github_user
```

### Use Case 2: Agent Message Routing
```python
# Agent Ether routing notification
user_email = context.get("user_email")
slack_id = pantheon.resolve(user_email, target="slack")
slack.notify(slack_id, "Task complete")
```

### Use Case 3: Provenance Tracking
```bash
# Git commit shows user_id=42, need email for attribution
user_id=42
email=$(mapper resolve mysql://users/$user_id --to contacts)
echo "Modified by: $email"
```

### Use Case 4: Universal Search
```bash
# Find all mentions of a user across all systems
for identity in $(mapper discover alice@example.com); do
    tia search all "$identity"
done
```
