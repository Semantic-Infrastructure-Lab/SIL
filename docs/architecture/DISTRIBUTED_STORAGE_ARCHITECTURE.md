---
title: "Distributed Storage Architecture for SIL"
subtitle: "IPFS Integration Strategy for Semantic Memory, Identity, and Provenance"
category: architecture
project: SIL
tags: [distributed-systems, ipfs, architecture, semantic-memory, provenance, identity, agent-discovery]
author: TIA
created: 2025-12-10
status: research-planning
version: 0.1.0
beth_topics:
  - distributed-storage
  - ipfs
  - content-addressing
  - semantic-memory
  - provenance
  - agent-identity
  - genesisgraph
  - layer-0-architecture
  - decentralized-systems
quality:
  completeness: 85
  accuracy: 90
  freshness: 100
  practical_value: 85
related_docs:
  - docs/canonical/SIL_SEMANTIC_OS_ARCHITECTURE.md
  - docs/innovations/GENESISGRAPH.md
  - docs/canonical/TRUST_ASSERTION_PROTOCOL.md
  - docs/innovations/PANTHEON.md
  - docs/canonical/HIERARCHICAL_AGENCY_FRAMEWORK.md
---

# Distributed Storage Architecture for SIL

> *Content-addressed, decentralized infrastructure for semantic memory, identity, and provenance*

**Status:** Research & Planning
**Version:** 0.1.0
**Last Updated:** 2025-12-10
**Author:** TIA (with Scott Senchak)

---

## TL;DR

**Question:** Should SIL use internet-scale distributed file storage (IPFS) for identity, provenance, and agent discovery?

**Answer:** **YES** - Content-addressed distributed storage is architecturally aligned with SIL's core principles and provides critical capabilities for:

1. **Semantic Memory (Layer 0)** - Cryptographic identity for knowledge artifacts
2. **Provenance (GenesisGraph)** - Immutable, verifiable artifact lineage
3. **Identity (DIDs)** - Decentralized agent identity and credential storage
4. **Agent Discovery** - Peer-to-peer capability registry without central authority

**Strongest case:** Layer 0 (Semantic Memory) + GenesisGraph integration. Start here.

**Implementation:** Phased approach over 12 months, beginning with IPFS backend for Beth's knowledge mesh.

---

## Table of Contents

1. [Overview](#overview)
2. [Current SIL Architecture Context](#current-sil-architecture-context)
3. [Use Case Analysis](#use-case-analysis)
   - [Layer 0: Semantic Memory](#1-layer-0-semantic-memory---strongest-case)
   - [Identity (DIDs + IPFS)](#2-identity-dids--ipfs---medium-high-priority)
   - [Provenance (GenesisGraph + IPFS)](#3-provenance-genesisgraph--ipfs---high-synergy)
   - [Agent Discovery](#4-agent-discovery---medium-priority)
4. [Where IPFS is Less Critical](#where-ipfs-is-less-critical)
5. [Phased Implementation Strategy](#phased-implementation-strategy)
6. [Technical Architecture](#technical-architecture)
7. [Key Synergies](#key-architectural-synergies)
8. [Open Questions](#open-questions)
9. [References](#references)

---

## Overview

This document evaluates **content-addressed distributed storage** (IPFS and similar systems) as infrastructure for SIL's Semantic OS.

**Core Thesis:** SIL's architectural commitments to provenance, verifiability, and decentralized collaboration align naturally with content-addressed storage systems like IPFS.

**Key Alignment Points:**
- **Content-addressing** matches GenesisGraph's hash-based provenance commitments
- **Immutability** supports verifiable knowledge graphs and audit trails
- **Decentralization** enables agent-to-agent coordination without central authorities
- **Cryptographic identity** (CIDs) provides unforgeable references to artifacts

**Strategic Value:**
- Moves SIL from "centralized semantic infrastructure" to "distributed semantic infrastructure"
- Enables true peer-to-peer agent collaboration
- Provides censorship-resistant knowledge preservation
- Natural fit for multi-organization collaboration (SIL ecosystem partners)

---

## Current SIL Architecture Context

### Existing Architecture References

From **SIL_SEMANTIC_OS_ARCHITECTURE.md (Layer 0: Semantic Memory)**:
> "Storage Engines:
>   - Content-addressable storage (IPFS-like)"

**Already planned** - this document provides implementation strategy and prioritization.

### Existing Provenance Infrastructure

**GenesisGraph v0.3.0** (from GENESISGRAPH.md):
- Merkle tree commitments for sealed subgraphs
- Cryptographic hash-based lineage
- Selective disclosure (A/B/C levels)
- DID support (did:key, did:web, did:ion, did:ethr)

**Natural IPFS synergy** - Merkle DAGs are IPFS's native data structure.

### Current Knowledge Mesh Scale

**Beth Knowledge Graph** (from tia-boot output):
- 15,327 indexed files
- 38,084 keywords
- S3 sync for distributed team access

**Pain point:** Centralized S3 vs. decentralized IPFS for collaboration.

---

## Use Case Analysis

### 1. Layer 0: Semantic Memory - STRONGEST CASE

**Priority:** **HIGH** ⭐⭐⭐⭐⭐
**Complexity:** Medium
**Timeline:** Phase 1 (0-6 months)

#### Why Content-Addressing for Semantic Memory?

**Current Beth Architecture:**
```bash
# Current: Path-based references
/home/scottsen/src/tia/projects/SIL/docs/canonical/SIL_GLOSSARY.md

# Limitations:
# - Breaks when files move
# - No cryptographic integrity
# - Can't verify "this is the same document I read yesterday"
# - Difficult to share across organizations
```

**IPFS-Enhanced Beth Architecture:**
```yaml
# Content-addressed knowledge artifact
knowledge_node:
  cid: "bafybeigdyrzt5sfp7udm7hu76uh7y26nf3efuylqabf3oclgtqy55fbzdi"
  type: canonical_document
  title: "SIL Glossary"
  local_path: "projects/SIL/docs/canonical/SIL_GLOSSARY.md"
  content_hash: "sha256:abc123..."

  # Semantic relationships
  related_concepts:
    - cid: "bafybei.../PANTHEON.md"
      relationship: "defines-types-for"
    - cid: "bafybei.../GENESISGRAPH.md"
      relationship: "references-provenance-from"

  # Provenance
  provenance_chain:
    - previous_version: "bafybei.../SIL_GLOSSARY_v1.md"
      operation: "semantic_enrichment"
      agent: "did:key:z6Mk..."
      timestamp: "2025-12-09T10:27:00Z"
```

#### Benefits

**1. Cryptographic Identity for Knowledge**
- Every document has unforgeable CID
- "Read this specific version" = `ipfs get bafybei...`
- Version history becomes Merkle DAG (provenance built-in)

**2. Deduplication at Scale**
- Same content = same hash = single storage
- Beth indexes 15K+ files → significant storage savings
- Cross-project knowledge reuse without duplication

**3. Distributed Team Collaboration**
- Replace S3 sync with IPFS pinning
- No central authority controls knowledge access
- Partners can host their own IPFS nodes

**4. Verifiable Knowledge Graphs**
- Semantic relationships reference CIDs, not paths
- Can verify: "Does this relationship still point to the same content?"
- Audit trail: "What version of the glossary was used for this analysis?"

**5. Historical Queries**
- "Show me the architecture as of December 2025" = retrieve specific CID
- Time-travel through knowledge evolution
- Perfect reproducibility for research

#### Implementation Approach

**Storage Strategy:**
```python
# Hybrid storage model
class SemanticMemoryStore:
    def __init__(self):
        self.local_fs = FileSystemStore("/home/scottsen/src/tia")
        self.ipfs = IPFSStore()  # ipfs daemon
        self.cache = ContentAddressedCache()

    def store_knowledge(self, content: bytes, metadata: dict) -> str:
        # Always store locally for performance
        local_path = self.local_fs.write(content)

        # Compute CID without uploading
        cid = self.ipfs.compute_cid(content)

        # Optionally pin to IPFS for sharing
        if metadata.get("shareable", False):
            self.ipfs.add_and_pin(content, cid)

        # Index with both path and CID
        self.cache.index(cid=cid, path=local_path, metadata=metadata)
        return cid
```

**Beth Integration:**
```bash
# Enhanced Beth search
tia beth explore "provenance" --format cids
# Returns:
# bafybei.../GENESISGRAPH.md (score: 0.95)
# bafybei.../TRUST_ASSERTION_PROTOCOL.md (score: 0.87)

# Retrieve by CID (works anywhere)
tia beth get bafybei.../GENESISGRAPH.md
# → Fetches from local cache OR IPFS network

# Verify document integrity
tia beth verify bafybei.../GENESISGRAPH.md
# ✅ Content matches CID: bafybei...
```

#### Success Metrics

- **Deduplication ratio:** >30% storage reduction across knowledge mesh
- **Retrieval speed:** <100ms for cached CIDs, <3s for IPFS fetches
- **Integrity checks:** 100% of documents verifiable by CID
- **Collaboration:** 3+ organizations sharing knowledge via IPFS within 12 months

---

### 2. Identity (DIDs + IPFS) - MEDIUM-HIGH PRIORITY

**Priority:** **MEDIUM-HIGH** ⭐⭐⭐⭐
**Complexity:** High
**Timeline:** Phase 2 (3-9 months)

#### Current DID Support

From **GENESISGRAPH.md**:
> "90% DID support - Multi-method decentralized identity (did:key, did:web, did:ion, did:ethr)"

**Gap:** No `did:ipfs` method for storing DID documents on IPFS.

#### DID:IPFS Method Specification

**Standard DID Document on IPFS:**
```json
{
  "@context": [
    "https://www.w3.org/ns/did/v1",
    "https://w3id.org/security/suites/ed25519-2020/v1"
  ],
  "id": "did:ipfs:bafybeigdyrzt5sfp7udm7hu76uh7y26nf3efuylqabf3oclgtqy55fbzdi",

  "verificationMethod": [{
    "id": "did:ipfs:bafybei...#key-1",
    "type": "Ed25519VerificationKey2020",
    "controller": "did:ipfs:bafybei...",
    "publicKeyMultibase": "z6MkpTHR8VNsBxYAAWHut2Geadd9jSwuBV8xRoAnwWsdvktH"
  }],

  "authentication": ["#key-1"],
  "assertionMethod": ["#key-1"],

  "service": [{
    "id": "#semantic-passport",
    "type": "SemanticPassport",
    "serviceEndpoint": "ipfs://bafybei.../passport.json"
  }, {
    "id": "#agent-capabilities",
    "type": "AgentCapabilities",
    "serviceEndpoint": "ipns://k51qzi5uqu5dlvj2baxnqndepeb86cbk3ng7n3i46uzyxzyqj2xjonzllnv0v8"
  }]
}
```

**Published to IPFS:**
```bash
# Create DID document
ipfs add did-document.json
# → bafybeigdyrzt5sfp7udm7hu76uh7y26nf3efuylqabf3oclgtqy55fbzdi

# DID identifier = IPFS CID
did:ipfs:bafybeigdyrzt5sfp7udm7hu76uh7y26nf3efuylqabf3oclgtqy55fbzdi
```

#### Semantic Passports as IPFS Objects

**Trust Assertion Bundle (from TRUST_ASSERTION_PROTOCOL.md):**
```json
{
  "@context": "https://sil.org/schemas/semantic-passport/v1",
  "id": "ipfs://bafybei.../passport-bob-2025.json",
  "subject": "did:ipfs:bafybei.../bob-did.json",

  "assertions": [
    {
      "id": "tap:assertion:uuid-1",
      "issuer": "did:ipfs:bafybei.../alice-did.json",
      "claim": {
        "type": "has-capability",
        "value": "distributed-systems",
        "level": "expert"
      },
      "proof": {
        "type": "Ed25519Signature2020",
        "verificationMethod": "did:ipfs:bafybei.../alice-did.json#key-1",
        "proofValue": "z3MvGc..."
      },
      "provenance": {
        "graph_node": "ipfs://bafybei.../genesisgraph-node-123.json"
      }
    }
  ],

  "metadata": {
    "issued": "2025-01-01T00:00:00Z",
    "valid_until": "2026-01-01T00:00:00Z",
    "cid": "bafybei.../passport-bob-2025.json"
  }
}
```

#### Benefits

**1. Agent Identity Persistence**
- DID documents immutably stored on IPFS
- Agents can present verifiable credentials anywhere
- No reliance on centralized identity providers

**2. Decentralized DID Resolution**
```python
def resolve_did(did: str) -> DIDDocument:
    """Resolve DID to DID document via IPFS"""
    if did.startswith("did:ipfs:"):
        cid = did.split(":")[-1]
        content = ipfs.get(cid)
        return DIDDocument.parse(content)
    # ... other DID methods
```

**3. Verifiable Credential Chains**
- Semantic Passports reference other IPFS objects
- Full provenance chain retrievable via CID references
- Cryptographic verification of entire credential graph

**4. Cross-Organization Trust**
- Organization A issues credential → stored on IPFS
- Organization B verifies credential → fetches from IPFS
- No shared infrastructure required

#### Implementation Approach

**DID Method Handler:**
```python
# GenesisGraph DID resolver extension
class IPFSDIDResolver:
    def resolve(self, did: str) -> DIDDocument:
        cid = self._extract_cid(did)

        # Try local cache first
        if cached := self.cache.get(cid):
            return DIDDocument.parse(cached)

        # Fetch from IPFS network
        content = self.ipfs.get(cid, timeout=5)

        # Cache for future lookups
        self.cache.set(cid, content)

        return DIDDocument.parse(content)
```

**Trust Assertion Protocol Integration:**
```bash
# Agent publishes semantic passport
tia agent passport publish --to-ipfs
# → Stores passport as IPFS object
# → Returns CID for sharing

# Verifier checks passport
tia agent passport verify ipfs://bafybei.../passport.json
# → Fetches from IPFS
# → Verifies all signatures
# → Checks GenesisGraph provenance chains
# ✅ Passport valid, all assertions verified
```

#### Success Metrics

- **DID resolution latency:** <2s for IPFS DIDs
- **Passport verification:** 100% cryptographic verification pass rate
- **Adoption:** 10+ agent identities using did:ipfs within 9 months

---

### 3. Provenance (GenesisGraph + IPFS) - HIGH SYNERGY

**Priority:** **HIGH** ⭐⭐⭐⭐⭐
**Complexity:** Medium
**Timeline:** Phase 1-2 (0-9 months)

#### The Perfect Match: Merkle DAGs + IPFS

**GenesisGraph Core (from GENESISGRAPH.md):**
> "Merkle Tree Provenance Commitments:
>   - Hash-only lineage for proprietary pipeline segments
>   - Selective exposure of input/output digests
>   - Optional inclusion proofs without revealing full tree"

**IPFS Core:**
- Native Merkle DAG data structure
- Content-addressed by hash
- Built-in cryptographic integrity

**Synergy:** GenesisGraph's provenance model IS a Merkle DAG. IPFS is the natural storage layer.

#### Enhanced GenesisGraph with IPFS Artifacts

**Current GenesisGraph (file-based):**
```yaml
operations:
  - id: train_model
    tool: pytorch
    parameters:
      learning_rate: 0.001
    inputs:
      - path: /local/training_data.parquet
    outputs:
      - path: /local/model_v1.pt
```

**IPFS-Enhanced GenesisGraph:**
```yaml
operations:
  - id: train_model
    tool: pytorch
    parameters:
      learning_rate: 0.001

    # Inputs/outputs are IPFS CIDs
    inputs:
      - cid: bafybei.../training_data.parquet
        local_path: /cache/training_data.parquet  # optional cache

    outputs:
      - cid: bafybei.../model_v1.pt
        local_path: /cache/model_v1.pt

    # Provenance metadata also on IPFS
    provenance:
      graph_cid: bafybei.../operation-train-model.json
      parent_operations:
        - bafybei.../operation-preprocess.json
```

#### Sealed Subgraph Storage on IPFS

**Level C: Sealed Subgraph (from GENESISGRAPH.md:66):**
```yaml
# Proprietary pipeline sealed as Merkle root
sealed_subgraph:
  # Root hash = IPFS CID of sealed pipeline
  root_cid: "bafybei.../proprietary-training-pipeline.sealed"

  inputs:
    - cid: "bafybei.../raw_data.parquet"

  outputs:
    - cid: "bafybei.../final_model.pt"

  policies:
    - claim: "FDA 21 CFR Part 11 compliant"
      signature: "..."
      proof_cid: "bafybei.../fda-compliance-proof.json"
```

**What This Enables:**

1. **Universal Artifact Addressing**
   - `ipfs get bafybei.../model_v1.pt` works anywhere
   - No path dependencies, no centralized storage

2. **Reproducibility Across Machines**
   - GenesisGraph references artifacts by CID
   - Replay pipeline on any machine with IPFS

3. **Regulatory Compliance**
   - FDA auditor: "Verify this model training process"
   - Submit: GenesisGraph YAML with IPFS CIDs
   - Auditor fetches artifacts via IPFS, verifies hashes
   - No need to share proprietary infrastructure

4. **Collaboration Without Centralization**
   - Organization A produces model → IPFS
   - Organization B validates model → fetches from IPFS
   - No S3 buckets, no VPNs, no access control nightmares

#### Implementation Strategy

**GenesisGraph IPFS Backend:**
```python
# genesisgraph/storage/ipfs_backend.py
class IPFSArtifactStore:
    """Store and retrieve GenesisGraph artifacts via IPFS"""

    def store_artifact(self, file_path: str, metadata: dict) -> str:
        """Store artifact and return CID"""
        with open(file_path, 'rb') as f:
            content = f.read()

        # Add to IPFS
        result = self.ipfs.add(content, pin=True)
        cid = result['Hash']

        # Store metadata mapping
        self.metadata_store.set(cid, metadata)

        return cid

    def retrieve_artifact(self, cid: str, cache_path: str = None) -> bytes:
        """Retrieve artifact by CID, optionally cache locally"""
        content = self.ipfs.get(cid)

        if cache_path:
            with open(cache_path, 'wb') as f:
                f.write(content)

        return content
```

**Enhanced GenesisGraph CLI:**
```bash
# Store operation artifacts to IPFS
genesisgraph store-artifacts workflow.gg.yaml --backend ipfs
# → Uploads all input/output files to IPFS
# → Rewrites workflow YAML with CIDs
# → Saves as workflow.gg.ipfs.yaml

# Retrieve and verify
genesisgraph retrieve-artifacts workflow.gg.ipfs.yaml --to /cache
# → Downloads all artifacts from IPFS
# → Verifies hashes match CIDs
# ✅ All artifacts verified

# Verify without downloading (efficiency!)
genesisgraph verify workflow.gg.ipfs.yaml
# → Checks IPFS DHT for artifact availability
# → Verifies Merkle tree integrity
# ✅ Workflow valid, all artifacts present on network
```

#### Success Metrics

- **Artifact availability:** >99% uptime via IPFS network
- **Verification speed:** <30s to verify full GenesisGraph workflow
- **Regulatory adoption:** 1+ FDA submission using IPFS-backed GenesisGraph within 12 months

---

### 4. Agent Discovery - MEDIUM PRIORITY

**Priority:** **MEDIUM** ⭐⭐⭐
**Complexity:** High
**Timeline:** Phase 3 (6-12 months)

#### The Agent Discovery Problem

**Current Gap:**
- Agent Ether coordinates agents (SIL_SEMANTIC_OS_ARCHITECTURE.md mentions choreography)
- No explicit mechanism for "find agents with capability X"
- Trust Assertion Protocol defines trust claims, but no discovery registry

**Traditional Solutions:**
- Central registry (defeats decentralization)
- Manual configuration (doesn't scale)
- Proprietary discovery protocols (vendor lock-in)

**IPFS Solution:** Decentralized capability registry via IPNS + DHT.

#### Architecture: IPNS-Based Agent Registry

**Agent Capability Document:**
```json
{
  "@context": "https://sil.org/schemas/agent-capabilities/v1",
  "agent_id": "did:ipfs:bafybei.../agent-bob.json",

  "capabilities": {
    "distributed-systems": {
      "level": "expert",
      "evidence_cid": "bafybei.../commits-analysis.json",
      "tap_assertions": [
        "ipfs://bafybei.../assertion-alice-endorses-bob.json"
      ]
    },
    "semantic-infrastructure": {
      "level": "advanced",
      "evidence_cid": "bafybei.../papers-authored.json"
    }
  },

  "availability": {
    "endpoints": [
      "libp2p://QmPeerID...",
      "https://agent-bob.example.com"
    ],
    "protocols": ["tap-v1", "hierarchical-agency-v1"],
    "status": "available"
  },

  "metadata": {
    "published": "2025-12-10T00:00:00Z",
    "version": "1.2.0",
    "cid": "bafybei.../agent-bob-capabilities.json"
  }
}
```

**Published via IPNS (Mutable Pointer):**
```bash
# Agent publishes capabilities
ipfs add agent-bob-capabilities.json
# → bafybei.../agent-bob-capabilities.json (immutable)

# Publish to IPNS (mutable name)
ipfs name publish bafybei.../agent-bob-capabilities.json
# → Published to IPNS name: k51qzi5uqu5dlvj2baxnqndepeb86cbk3ng7n3i46uzyxzyqj2xjonzllnv0v8

# Now anyone can resolve:
ipfs name resolve k51qzi5uqu5dlvj2baxnqndepeb86cbk3ng7n3i46uzyxzyqj2xjonzllnv0v8
# → /ipfs/bafybei.../agent-bob-capabilities.json (latest version)
```

#### Discovery Flow

**1. Agent Publishes Capabilities**
```bash
tia agent publish-capabilities --to-ipfs
# → Creates capability document
# → Adds to IPFS (immutable CID)
# → Publishes to IPNS (mutable pointer tied to agent's key)
# → Announces to DHT with tags: ["distributed-systems", "expert"]
```

**2. Agent Discovery via DHT Query**
```bash
tia agent discover --capability "distributed-systems" --level "expert"
# → Queries IPFS DHT for matching agents
# → Returns IPNS names of matching agents
# → Resolves IPNS → latest capability documents
# → Verifies trust assertions via GenesisGraph
# → Returns ranked list of agents

# Results:
# 1. agent-bob (did:ipfs:bafybei...bob)
#    - Capability: distributed-systems (expert)
#    - Endorsed by: alice, charlie
#    - Availability: online
#    - Endpoint: libp2p://QmBob...
#
# 2. agent-eve (did:ipfs:bafybei...eve)
#    - Capability: distributed-systems (expert)
#    - Endorsed by: alice
#    - Availability: offline (last seen: 2h ago)
```

**3. Trust Verification**
```python
def verify_agent_capability(agent_did: str, capability: str) -> bool:
    """Verify agent's claimed capability via TAP assertions"""

    # Resolve agent's IPNS name → capability document
    cap_doc = resolve_agent_capabilities(agent_did)

    # Get claimed capability
    claim = cap_doc['capabilities'].get(capability)
    if not claim:
        return False

    # Fetch and verify all TAP assertions
    for assertion_cid in claim['tap_assertions']:
        assertion = ipfs.get(assertion_cid)

        # Verify signature
        if not verify_tap_signature(assertion):
            return False

        # Verify provenance chain via GenesisGraph
        provenance_cid = assertion['provenance']['graph_cid']
        if not verify_genesis_graph(provenance_cid):
            return False

    return True
```

**4. Agent-to-Agent Communication (libp2p)**
```python
# Agent Ether delegates task to discovered agent
async def delegate_task(task: Task, agent_did: str):
    # Discover agent endpoint
    cap_doc = resolve_agent_capabilities(agent_did)
    libp2p_endpoint = cap_doc['availability']['endpoints'][0]

    # Connect via libp2p
    conn = await libp2p.connect(libp2p_endpoint)

    # Verify agent identity (DID challenge-response)
    if not await verify_agent_identity(conn, agent_did):
        raise UnauthorizedAgent(agent_did)

    # Delegate task using hierarchical agency protocol
    result = await conn.send_task(task)
    return result
```

#### Benefits

**1. No Central Registry**
- Agents self-publish to IPFS DHT
- No single point of failure
- No gatekeeper controls who can be an agent

**2. Cryptographic Identity**
- IPNS keys = agent identity
- Can't spoof another agent's capabilities
- DID-based authentication

**3. Offline-First**
- Capabilities cached locally
- DHT provides eventual consistency
- Works in low-connectivity environments

**4. Censorship Resistance**
- No central authority can delist an agent
- Agents can migrate between IPFS networks
- Perfect for multi-organization collaboration

#### Implementation Approach

**Phase 3a: Basic IPNS Publishing (Months 6-8)**
```python
# tia/lib/agent/ipfs_registry.py
class IPFSAgentRegistry:
    def publish_capabilities(self, agent_did: str, capabilities: dict):
        """Publish agent capabilities to IPFS + IPNS"""

        # Create capability document
        cap_doc = {
            "agent_id": agent_did,
            "capabilities": capabilities,
            "published": datetime.utcnow().isoformat()
        }

        # Add to IPFS
        cid = self.ipfs.add_json(cap_doc)

        # Publish to IPNS
        ipns_name = self.ipfs.name.publish(cid, key=agent_did)

        return ipns_name
```

**Phase 3b: DHT Discovery (Months 8-10)**
```python
# Enhanced discovery with DHT queries
class AgentDiscovery:
    def discover(self, capability: str, level: str = None) -> List[Agent]:
        # Query IPFS DHT for matching agents
        # This requires custom DHT provider records
        matches = self.ipfs.dht.findprovs(
            key=f"/agent-capability/{capability}/{level or 'any'}"
        )

        # Resolve each IPNS name
        agents = []
        for match in matches:
            cap_doc = self.resolve_ipns(match['ipns_name'])
            agents.append(Agent.from_capability_doc(cap_doc))

        return agents
```

**Phase 3c: Trust Verification Integration (Months 10-12)**
```bash
# Full discovery with trust verification
tia agent discover \
  --capability "distributed-systems" \
  --level "expert" \
  --require-endorsements 2 \
  --verify-provenance

# → Queries DHT
# → Resolves capability documents
# → Fetches TAP assertions from IPFS
# → Verifies GenesisGraph provenance chains
# → Returns only agents passing all checks
```

#### Success Metrics

- **Discovery latency:** <5s to find and verify agents
- **Network coverage:** >95% of agents discoverable via DHT
- **Trust verification:** 100% of returned agents pass provenance checks
- **Adoption:** 20+ agents using IPFS discovery within 12 months

---

## Where IPFS is Less Critical

### Layer 4: Deterministic Engines (Morphogen)

**Why NOT IPFS:**
- Morphogen requires hermetic, reproducible execution
- IPFS has non-deterministic network latency
- Pinning reliability varies across nodes
- Execution timing must be predictable

**Better Solution:**
- Local content-addressed store (Nix-style)
- IPFS as **distribution layer** (fetch once, cache forever)
- Deterministic builds use local cache

**Hybrid Approach:**
```bash
# Fetch Morphogen operator dependencies via IPFS
morphogen fetch-deps operator.yaml --via ipfs
# → Downloads to local content-addressed cache
# → Verifies hashes
# → Subsequent executions use local cache (deterministic)

# Build uses local cache only
morphogen build operator.yaml
# → No network calls during build
# → Reproducible execution
```

### Layer 5: Human Interfaces

**Why NOT IPFS (generally):**
- CLIs, GUIs don't need decentralization
- Users expect fast, local responses
- IPFS latency too high for interactive UIs

**Exception - Public Documentation:**
```bash
# SIL documentation could be IPFS-hosted
https://sil.org/docs → IPNS gateway
# → Censorship-resistant
# → Distributed hosting (multiple pinners)
# → Verifiable integrity (CID in URL)
```

---

## Team & Skills Requirements

### Recommended Team Lead

**Kelly Lynch** - Lead Engineer, Identity & Trust Systems ⭐⭐⭐⭐⭐

**Why Kelly is the Ideal Lead:**

1. **DocuSign Experience** (6 years, current)
   - Digital signatures, cryptographic verification at enterprise scale
   - Trust infrastructure for billions of legally-binding signatures
   - **Direct translation** to DID verification, Trust Assertions, Semantic Passports

2. **Distributed Systems Depth**
   - AWS EC2 Spot (3 years): Cloud infrastructure at scale
   - Microsoft Windows Server (22 years): Platform infrastructure
   - Understands content-addressed storage, distributed coordination, fault tolerance

3. **Code Craftsmanship - Strategic Asset**
   - **Narrative code style**: "Summary at top, conclusion at bottom, no surprises"
   - Critical for identity/cryptography code (must be auditable, verifiable, maintainable)
   - Code that lasts decades (Windows Server 2012 still running in production)
   - Sets engineering quality standard for SIL team

4. **Personal Trust**
   - Friend of SIL founder (Scott Senchak), former Microsoft colleague
   - Direct experience with code quality, shipping discipline
   - Warm relationship = fast engagement, mutual understanding

**Recommended Assignment:**
- **Primary**: Phase 2 (DID:IPFS + Trust Assertions) - leverages DocuSign expertise
- **Secondary**: Phase 1 (IPFS storage backend) - builds IPFS foundation
- **Advanced**: Phase 3 (Agent discovery) - after establishing IPFS/DID comfort

**See:** `/team/personnel/candidates/kelly_lynch.md` for full profile and assessment
**Role Spec:** `/team/hiring/lead-engineer-identity-trust.md` for detailed role description

---

### Required Skills Profile

**For Phase 1-2 (Critical Path):**

| Skill Domain | Required Level | Why Critical | Kelly's Fit |
|--------------|----------------|--------------|-------------|
| **Cryptographic Identity** | Expert | DID verification, signature validation, trust chains | ⭐⭐⭐⭐⭐ (DocuSign) |
| **Distributed Systems** | Advanced | IPFS, DHT, eventual consistency, fault tolerance | ⭐⭐⭐⭐ (AWS, Microsoft) |
| **Content-Addressed Storage** | Intermediate+ | IPFS/IPLD, Merkle DAGs, hash verification | ⭐⭐⭐ (learnable, strong foundation) |
| **Platform Infrastructure** | Expert | Production systems, SDKs, long-term maintenance | ⭐⭐⭐⭐⭐ (22 years Microsoft) |
| **Code Quality** | Expert | Auditable, maintainable, narrative style | ⭐⭐⭐⭐⭐ (proven track record) |

**Secondary Skills (Valuable):**
- Python (primary implementation language)
- TypeScript/JavaScript (SDK development)
- Zero-knowledge proofs (for selective disclosure)
- Regulatory compliance (FDA, ISO standards)

---

### Code Quality Expectations

**The "Narrative Code" Standard** (inspired by Kelly's approach):

**What We Expect:**
```python
class DIDIPFSResolver:
    """
    Resolve DID:IPFS identifiers to verified DID documents.

    Trust chain: DID identifier → IPFS CID → Content verification → DID document

    Security: All hashes verified, all signatures checked, all errors explicit.
    """

    def resolve(self, did: str) -> DIDDocument:
        """
        Resolve DID to document with cryptographic verification.

        Steps:
        1. Extract CID from DID identifier
        2. Fetch content from IPFS network
        3. Verify content hash matches CID (integrity)
        4. Parse and validate DID document (correctness)
        5. Return verified document

        Raises:
            InvalidDIDError: DID format invalid
            ContentHashMismatch: IPFS content doesn't match CID
            DocumentValidationError: DID document fails validation
        """
        cid = self._extract_cid_from_did(did)
        content = self._fetch_from_ipfs(cid)
        self._verify_hash_matches_cid(content, cid)
        document = self._parse_did_document(content)
        self._validate_did_document(document)
        return document
```

**Why This Matters:**
- ✅ **Auditable**: Regulators/security researchers can verify correctness
- ✅ **Maintainable**: Code survives 20+ years (identity infrastructure timeline)
- ✅ **Self-documenting**: Implementation IS the specification
- ✅ **No surprises**: Every step explicit, every error anticipated

**This is SIL Core Principle #5 (Pit of Success)** - right way = easy way.

---

### Team Growth Path

**Phase 1 (Months 0-6): Solo + Collaboration**
- **Lead Engineer** (Kelly): DID/IPFS implementation
- **Collaboration**: GenesisGraph team (provenance integration)
- **Collaboration**: Beth team (knowledge mesh integration)

**Phase 2 (Months 6-12): Small Team**
- **Lead Engineer**: Architecture, DID:IPFS, TAP specification
- **Backend Engineer**: IPFS infrastructure, storage optimization
- **Collaboration**: External contributors (open source community)

**Phase 3 (Months 12-18): Core Team**
- **Lead Engineer**: Architecture, standards engagement (W3C, IETF)
- **Identity Engineer**: DID methods, credential verification
- **Storage Engineer**: IPFS operations, DHT optimization
- **Open Source**: Community contributors on SDK development

**Goal:** Build infrastructure that **scales to thousands of external developers**, not just SIL team.

---

## Phased Implementation Strategy

### Phase 1: Foundation (Months 0-6)

**Goal:** IPFS backend for Layer 0 Semantic Memory

**Deliverables:**
1. **IPFS Storage Adapter for Beth**
   - Content-addressed indexing
   - Hybrid local + IPFS storage
   - CID-based document references

2. **GenesisGraph IPFS Integration**
   - Artifact storage via IPFS
   - CID-based provenance graphs
   - Verification without downloading

**Success Criteria:**
- ✅ Beth indexes 1000+ documents with CIDs
- ✅ GenesisGraph workflows reference IPFS artifacts
- ✅ <2s latency for cached documents

**Team Size:** 1-2 developers
**Estimated Effort:** 3-4 months development + 2 months testing

---

### Phase 2: Identity & Trust (Months 3-9)

**Goal:** DID:IPFS method + Semantic Passports on IPFS

**Deliverables:**
1. **DID:IPFS Method Implementation**
   - DID resolver for IPFS DIDs
   - DID document publishing to IPFS
   - Integration with GenesisGraph DID support

2. **Semantic Passports as IPFS Objects**
   - TAP assertions stored on IPFS
   - Trust bundles content-addressed
   - Credential verification via IPFS

**Success Criteria:**
- ✅ 10+ agents using did:ipfs identities
- ✅ 100+ TAP assertions stored on IPFS
- ✅ <3s latency for passport verification

**Team Size:** 1-2 developers
**Estimated Effort:** 4-5 months development + 2 months integration

---

### Phase 3: Agent Discovery (Months 6-12)

**Goal:** Decentralized agent capability registry via IPFS DHT

**Deliverables:**
1. **IPNS-Based Capability Publishing**
   - Agents publish capabilities to IPNS
   - Mutable pointers for capability updates
   - DHT announcement of capabilities

2. **Discovery Protocol Implementation**
   - DHT query for capability matching
   - Trust verification integration
   - Agent ranking and selection

3. **libp2p Agent Communication**
   - Peer-to-peer agent coordination
   - DID-based authentication
   - Hierarchical agency protocol over libp2p

**Success Criteria:**
- ✅ 20+ agents discoverable via DHT
- ✅ <5s discovery + verification latency
- ✅ 3+ organizations using decentralized discovery

**Team Size:** 2-3 developers (distributed systems expertise required)
**Estimated Effort:** 6-7 months development + 2 months piloting

---

## Technical Architecture

### System Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                     SIL Semantic OS                             │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  Layer 5: Human Interfaces                                      │
│  ┌────────────────────────────────────────────────────────┐    │
│  │  tia beth explore → IPFS CIDs                          │    │
│  │  tia agent discover → IPNS resolution                  │    │
│  └────────────────────────────────────────────────────────┘    │
│                                                                 │
│  Layer 3: Agent Ether (Multi-Agent Coordination)                │
│  ┌────────────────────────────────────────────────────────┐    │
│  │  Agent Discovery: Query IPFS DHT for capabilities     │    │
│  │  Trust Verification: Fetch TAP assertions from IPFS   │    │
│  │  Communication: libp2p peer-to-peer                    │    │
│  └────────────────────────────────────────────────────────┘    │
│                                                                 │
│  Layer 1: Pantheon IR (Semantic Types)                          │
│  ┌────────────────────────────────────────────────────────┐    │
│  │  TAP Assertion Type (stored as IPFS objects)          │    │
│  │  DID Document Type (stored on IPFS)                   │    │
│  └────────────────────────────────────────────────────────┘    │
│                                                                 │
│  Layer 0: Semantic Memory (Knowledge Storage)                   │
│  ┌────────────────────────────────────────────────────────┐    │
│  │  Beth Knowledge Graph: Documents indexed by CID       │    │
│  │  GenesisGraph: Provenance graphs with IPFS artifacts  │    │
│  │  Storage: Hybrid local cache + IPFS network           │    │
│  └────────────────────────────────────────────────────────┘    │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
                              │
                              │ Storage Layer
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                    IPFS Network Layer                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐         │
│  │ Local IPFS   │  │ Organization │  │  Public      │         │
│  │ Node         │  │ Pinning      │  │  Gateways    │         │
│  │ (cache)      │  │ Services     │  │  (fallback)  │         │
│  └──────────────┘  └──────────────┘  └──────────────┘         │
│         │                 │                   │                │
│         └─────────────────┴───────────────────┘                │
│                           │                                    │
│                    IPFS DHT Network                            │
│                (Distributed Hash Table)                        │
│                                                                 │
│  Features:                                                     │
│  • Content addressing (CID-based)                              │
│  • Peer-to-peer retrieval                                     │
│  • Cryptographic verification                                 │
│  • Decentralized naming (IPNS)                                │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Component Integration

**Beth + IPFS:**
```python
class BethIPFSAdapter:
    """Adapt Beth knowledge mesh to IPFS storage"""

    def index_document(self, file_path: str, metadata: dict):
        # Read document content
        content = read_file(file_path)

        # Compute CID
        cid = ipfs.add(content, only_hash=True)

        # Index with both path and CID
        beth_index.add(
            path=file_path,
            cid=cid,
            keywords=metadata['beth_topics'],
            quality=metadata['quality']
        )

        # Optionally pin for sharing
        if metadata.get('shareable'):
            ipfs.pin.add(cid)
```

**GenesisGraph + IPFS:**
```python
class GenesisGraphIPFSBackend:
    """Store GenesisGraph artifacts on IPFS"""

    def create_operation(self, op_id: str, inputs: List[str], outputs: List[str]):
        # Store input/output files to IPFS
        input_cids = [self.store_file(f) for f in inputs]
        output_cids = [self.store_file(f) for f in outputs]

        # Create operation node with CID references
        operation = {
            "id": op_id,
            "inputs": [{"cid": cid} for cid in input_cids],
            "outputs": [{"cid": cid} for cid in output_cids],
            "timestamp": datetime.utcnow().isoformat()
        }

        # Store operation itself to IPFS
        op_cid = ipfs.add_json(operation)

        return op_cid
```

**Agent Discovery + IPFS:**
```python
class IPFSAgentDiscovery:
    """Discover agents via IPFS DHT"""

    async def discover_agents(self, capability: str) -> List[AgentProfile]:
        # Query DHT for agents advertising this capability
        ipns_names = await self.query_dht(capability)

        # Resolve each IPNS name to latest capability document
        agents = []
        for ipns_name in ipns_names:
            cid = await ipfs.name.resolve(ipns_name)
            cap_doc = await ipfs.get_json(cid)

            # Verify trust assertions
            if await self.verify_assertions(cap_doc):
                agents.append(AgentProfile.from_doc(cap_doc))

        return agents
```

---

## Key Architectural Synergies

### 1. GenesisGraph + IPFS = Verifiable Provenance at Scale

**The Match:**
- GenesisGraph: Merkle DAG provenance model
- IPFS: Native Merkle DAG storage

**The Synergy:**
```
GenesisGraph Operation DAG:
  operation_1 (CID: bafybei...001)
      ├─ input: data.csv (CID: bafybei...002)
      └─ output: result.json (CID: bafybei...003)
          │
          └─ operation_2 (CID: bafybei...004)
              ├─ input: result.json (CID: bafybei...003)  ← Same CID!
              └─ output: final.txt (CID: bafybei...005)

IPFS automatically deduplicates:
  bafybei...003 stored once, referenced twice
```

**Impact:**
- Storage efficiency: Deduplication of intermediate artifacts
- Verification simplicity: `ipfs get <cid>` verifies hash automatically
- Reproducibility: Entire provenance graph retrievable by root CID

---

### 2. Trust Assertions + IPFS = Decentralized Trust Infrastructure

**The Match:**
- TAP: Typed trust claims with provenance
- IPFS: Immutable, verifiable claim storage

**The Synergy:**
```
Trust Assertion (CID: bafybei...assertion-123):
  issuer: did:ipfs:bafybei...alice
  subject: did:ipfs:bafybei...bob
  claim: { type: has-capability, value: distributed-systems }
  provenance: { graph_cid: bafybei...genesisgraph-xyz }

All components stored on IPFS:
  ✅ Assertion itself: bafybei...assertion-123
  ✅ Issuer DID: bafybei...alice
  ✅ Subject DID: bafybei...bob
  ✅ Provenance graph: bafybei...genesisgraph-xyz

Verification = recursive CID fetching + hash verification
```

**Impact:**
- No centralized trust authority needed
- Cross-organization trust without shared infrastructure
- Full audit trail via IPFS provenance chains

---

### 3. Beth + IPFS = Distributed Semantic Web

**The Match:**
- Beth: Semantic knowledge graph with 15K+ documents
- IPFS: Content-addressed, distributed document storage

**The Synergy:**
```
Beth Semantic Relationship:
  Document A (CID: bafybei...glossary)
    ─ defines-types-for →
  Document B (CID: bafybei...pantheon)

Stored as semantic triple:
  <bafybei...glossary> <defines-types-for> <bafybei...pantheon>

Query: "Find all documents that define types for Pantheon"
  → Returns: bafybei...glossary (and any others)
  → CID guarantees it's the EXACT version referenced
```

**Impact:**
- Cross-project knowledge reuse without path dependencies
- Version-specific semantic queries ("as of Dec 2025")
- Distributed collaboration (each org pins their docs)

---

## Open Questions

### 1. Performance vs. Decentralization Trade-offs

**Question:** How much IPFS latency is acceptable for interactive workflows?

**Current Assumptions:**
- Cached CIDs: <100ms (local)
- IPFS network fetch: <3s (acceptable for non-interactive)
- DHT queries: <5s (acceptable for discovery)

**Investigation Needed:**
- Benchmark IPFS retrieval latency at scale (1K, 10K, 100K documents)
- Measure DHT query performance with varying agent counts
- Test hybrid caching strategies (local → LAN → IPFS)

**Mitigation:**
- Aggressive local caching (most queries hit cache)
- Predictive prefetching (Beth preloads likely-needed docs)
- LAN-local IPFS cluster for <10ms latency

---

### 2. IPFS Pinning Strategy

**Question:** Who pins what? How do we ensure availability?

**Options:**

**A. Centralized Pinning (Simple, Less Resilient)**
- SIL operates pinning service
- All documents pinned by SIL nodes
- Single point of failure if SIL infra goes down

**B. Distributed Pinning (Complex, More Resilient)**
- Each organization pins their own documents
- Pinning clusters for important shared documents
- Incentive mechanisms (Filecoin?) for long-term storage

**C. Hybrid (Pragmatic)**
- Critical infrastructure (DIDs, core docs) pinned by multiple parties
- Project-specific documents pinned by owning organization
- Fallback to public pinning services (Pinata, web3.storage)

**Recommendation:** Start with C (Hybrid), migrate toward B as ecosystem matures.

---

### 3. IPFS vs. Alternatives

**Question:** Is IPFS the right content-addressed storage, or should we consider alternatives?

**Alternatives:**

| System | Pros | Cons |
|--------|------|------|
| **IPFS** | Mature, large network, good tooling | Performance variability, pinning complexity |
| **Arweave** | Permanent storage, no pinning needed | Expensive, centralized consensus |
| **Filecoin** | Incentivized pinning, IPFS-compatible | Complex, higher cost |
| **Dat/Hypercore** | Efficient replication, mutable | Smaller network, less tooling |
| **Git (content-addressed)** | Simple, well-understood | Not designed for large-scale distribution |

**Recommendation:**
- **Start with IPFS** (best ecosystem, tooling, adoption)
- **Abstract storage layer** (can swap backends later)
- **Monitor alternatives** (especially Filecoin for long-term archival)

---

### 4. Data Privacy & Encryption

**Question:** How do we handle sensitive data on a public IPFS network?

**Solutions:**

**A. Encryption Before Storage**
```python
# Encrypt sensitive documents before adding to IPFS
encrypted = encrypt(content, key=agent_key)
cid = ipfs.add(encrypted)

# Only agents with decryption key can read
# CID reveals nothing about content
```

**B. Private IPFS Clusters**
```bash
# Organization-specific IPFS network
ipfs init --profile=private-network
# → Only authorized nodes can join
# → Documents not visible on public DHT
```

**C. Hybrid Approach**
- Public metadata (document title, tags, quality scores)
- Private content (encrypted, key distribution via TAP)
- Provenance public (GenesisGraph graphs are auditable)

**Recommendation:** C (Hybrid) - balances transparency with privacy.

---

### 5. Migration Path from Current Infrastructure

**Question:** How do we migrate existing Beth/GenesisGraph deployments to IPFS?

**Migration Strategy:**

**Phase 1: Dual-Write (Months 0-3)**
```python
# Write to both local FS and IPFS
def store_document(content, metadata):
    # Existing behavior
    local_path = fs.write(content)

    # New IPFS storage
    cid = ipfs.add(content)

    # Index both
    beth.index(path=local_path, cid=cid, metadata=metadata)
```

**Phase 2: Dual-Read (Months 3-6)**
```python
# Try IPFS first, fallback to local
def retrieve_document(identifier):
    if identifier.startswith('bafybei'):  # CID
        return ipfs.get(identifier)
    else:  # Path
        return fs.read(identifier)
```

**Phase 3: IPFS-Primary (Months 6-12)**
```python
# IPFS is primary, local cache secondary
def retrieve_document(cid):
    # Check cache
    if cached := cache.get(cid):
        return cached

    # Fetch from IPFS
    content = ipfs.get(cid)
    cache.set(cid, content)
    return content
```

**Phase 4: Deprecate Local-Only Paths (Months 12+)**
- All new documents CID-only
- Legacy path-based references redirected to CIDs
- Local storage becomes pure cache

---

## References

### SIL Architecture Documents
- [SIL Semantic OS Architecture](/foundations/SIL_SEMANTIC_OS_ARCHITECTURE) - 6-layer architecture
- [GenesisGraph Innovation](/systems/GENESISGRAPH) - Provenance with selective disclosure
- [Trust Assertion Protocol](/foundations/TRUST_ASSERTION_PROTOCOL) - Typed trust claims
- [SIL Core Principles](../../lab/architecture/SIL_CORE_PRINCIPLES) - Progressive disclosure, composability

### External Resources
- [IPFS Documentation](https://docs.ipfs.tech/) - InterPlanetary File System
- [IPNS Specification](https://docs.ipfs.tech/concepts/ipns/) - Mutable naming on IPFS
- [libp2p Documentation](https://docs.libp2p.io/) - Modular peer-to-peer networking
- [DID Core Specification](https://www.w3.org/TR/did-core/) - Decentralized Identifiers (W3C)
- [Merkle DAGs](https://docs.ipfs.tech/concepts/merkle-dag/) - IPFS data structure

### Related Research
- Scott's Background (FOUNDER_BACKGROUND.md): Distributed Systems Research at Microsoft (2001-2003) - Peer-to-peer infrastructure with cryptographic identity
- Influences (INFLUENCES_AND_ACKNOWLEDGMENTS.md): Merkle DAGs for provenance, GenesisGraph process provenance

---

## Document Status

**Version:** 0.1.0
**Status:** Research & Planning
**Next Review:** 2026-01-10 (1 month)

**Open Tasks:**
- [ ] Benchmark IPFS latency at Beth scale (15K+ documents)
- [ ] Design IPFS pinning strategy (centralized vs distributed)
- [ ] Prototype Beth IPFS adapter
- [ ] Prototype GenesisGraph IPFS backend
- [ ] Evaluate IPFS alternatives (Arweave, Filecoin)
- [ ] Design encryption strategy for sensitive data
- [ ] Create migration plan for existing deployments

**Contributors:**
- Scott Senchak (SIL Founder) - Architecture direction
- TIA (Chief Semantic Agent) - Document synthesis, research

**Feedback Welcome:**
- Technical review from distributed systems experts
- Privacy/security review for encryption strategy
- Performance benchmarking collaboration

---

**Version History:**
- v0.1.0 (2025-12-10): Initial research document analyzing IPFS integration points across SIL architecture
