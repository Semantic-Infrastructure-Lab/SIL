# Authorization Protocol

> *Permission Model for Multi-Agent Systems*

## TL;DR

**What is Authorization?** An OS-level primitive separating permission ("may do") from capability ("can do").

**Key insight:** Trust Assertion Protocol (TAP) proves an agent *can* perform a task. Authorization proves an agent *may* perform it with defined scope and constraints.

**The primitive:**
```python
AuthorizationGrant(
    principal,    # Who grants permission
    agent,        # Who receives permission
    scope,        # What actions allowed
    constraints,  # Budgets, limits, restrictions
    valid_until   # Temporal bounds
)
```

**Why it matters:** Multi-agent coordination requires explicit permission models. Capability ≠ authority.

---

## The Problem

Traditional systems conflate **capability** (what you *can* do) with **authorization** (what you *may* do):

**Without separation**:
```python
# Agent has deployment capability via TAP
if has_capability(agent, "deploy-production"):
    deploy()  # ❌ No permission check!
```

**Problems**:
- Agent with capability auto-granted permission (security risk)
- No scope limits (can deploy anything, anywhere)
- No budget constraints (unlimited resource consumption)
- No temporal bounds (permission never expires)
- No audit trail of grants (compliance failure)
- Cannot revoke permission without removing capability (inflexible)

---

## The Solution: Authorization as Separate Primitive

**Authorization extends TAP** with explicit permission grants:

```python
# Step 1: Check capability (TAP)
tap = query_tap(agent, "has-capability", "deploy-production")
if not tap:
    return Error("Agent lacks capability")

# Step 2: Check authorization (NEW primitive)
auth = query_authorization(agent, "deploy-production")
if not auth or auth.expired():
    return Error("Agent lacks permission")

# Step 3: Validate constraints
if exceeds_budget(task, auth.budget):
    return Error("Exceeds authorized budget")

# All checks pass → delegate
delegate(agent, task)
```

---

## OS Architecture Perspective

Every multi-agent operating system needs **two permission models**:

| Model | What It Proves | Semantic OS Primitive | Analogous To |
|-------|---------------|---------------------|-------------|
| **Capability Model** | Technical ability | TAP (Trust Assertion Protocol) | Unix: executable bit, process capabilities |
| **Permission Model** | Granted authority | AuthorizationGrant (this protocol) | Unix: file ownership, ACLs, sudo grants |

**Traditional OS Example**:
```bash
# User can execute /usr/bin/reboot (capability)
$ ls -l /usr/bin/reboot
-rwxr-xr-x 1 root root

# But needs sudo permission (authorization)
$ reboot
Permission denied

$ sudo reboot  # ✓ Has both capability AND authorization
```

**Semantic OS Example**:
```python
# Agent CAN deploy (TAP capability)
tap_assertion = { "type": "has-capability", "value": "deploy-production" }

# Agent MAY deploy (Authorization permission)
auth_grant = AuthorizationGrant(
    principal="did:user:alice",
    agent="did:agent:deployment-bot",
    scope=["deploy-production"],
    constraints={"max_instances": 10, "budget_usd": 1000},
    valid_until="2025-12-31T23:59:59Z"
)
```

---

## AuthorizationGrant Primitive

### Schema (Layer 1: Pantheon IR)

```python
from dataclasses import dataclass
from datetime import datetime
from typing import List, Dict, Optional

@dataclass
class AuthorizationGrant:
    """
    Explicit permission to perform actions with defined scope and constraints.

    Stored in GenesisGraph as typed edge: principal --[grants-auth]--> agent
    Checked by Agent Ether before delegation.
    """

    # Core fields
    grant_id: str                    # Unique identifier
    principal: str                   # DID of who grants (human or org)
    agent: str                       # DID of who receives (agent)
    scope: List[str]                 # What actions permitted

    # Constraints
    constraints: Dict[str, any]      # Budgets, limits, restrictions
    # Common constraints:
    #   budget_usd: float           # Dollar spending limit
    #   max_api_calls: int          # API call limit
    #   allowed_domains: List[str]  # Domain restrictions
    #   rate_limit: str             # "100/hour"

    # Temporal
    valid_from: datetime             # When permission starts
    valid_until: datetime            # When permission expires

    # Revocation
    revocable: bool = True           # Can be revoked?
    revoked_at: Optional[datetime] = None

    # Provenance
    granted_at: datetime             # When grant created
    provenance_node: str             # GenesisGraph node ID

    # Optional
    delegation_depth: int = 0        # How deep can subdelegate
    requires_confirmation: bool = False  # Needs user confirm per use?
```

### Lifecycle

```
┌─────────────┐
│   PENDING   │  Grant created, not yet active
└──────┬──────┘
       │
       ▼
┌─────────────┐
│   ACTIVE    │  valid_from ≤ now < valid_until
└──────┬──────┘
       │
       ▼
┌─────────────┐
│  EXPIRED    │  now ≥ valid_until (cannot be used)
└─────────────┘

       OR

┌─────────────┐
│   REVOKED   │  Principal revoked early (revoked_at set)
└─────────────┘
```

---

## Integration with Semantic OS Layers

### Layer 0: Semantic Memory

AuthorizationGrant stored as **typed GenesisGraph edge**:

```
(principal:DID) --[grants-authorization]--> (agent:DID)

Edge metadata:
  - grant_id
  - scope (list)
  - constraints (dict)
  - temporal bounds (valid_from, valid_until)
  - revocation status
  - provenance chain
```

### Layer 1: Pantheon IR

AuthorizationGrant is a **semantic type** in Pantheon IR vocabulary:
- Composes with TAP assertions
- Validated by domain modules
- Lifted/lowered across representations

### Layer 3: Agent Ether

Before delegation, Agent Ether **checks authorization**:

```python
async def delegate_task(agent_id: str, task: Task) -> Result:
    # 1. Check capability (TAP)
    if not await verify_capability(agent_id, task.required_capability):
        return Error("Agent lacks capability")

    # 2. Check authorization (NEW)
    auth = await query_authorization(agent_id, task.action)
    if not auth:
        return Error("No authorization grant found")

    if auth.expired():
        return Error("Authorization expired")

    if auth.revoked_at:
        return Error("Authorization was revoked")

    # 3. Validate constraints
    if not validate_constraints(task, auth.constraints):
        return Error("Task violates authorization constraints")

    # 4. Check delegation depth (if subdelegating)
    if task.delegation_chain_depth > auth.delegation_depth:
        return Error("Exceeds delegation depth limit")

    # All checks pass
    return await execute_with_provenance(agent_id, task, auth.grant_id)
```

### Layer 5: Interfaces

User grants authorization through UI:

```typescript
// User grants agent permission to deploy
const grant = await createAuthorizationGrant({
  agent: "did:agent:deployment-bot",
  scope: ["deploy-production", "rollback-production"],
  constraints: {
    budget_usd: 1000,
    max_instances: 10,
    allowed_regions: ["us-west-2"]
  },
  valid_until: "2025-12-31T23:59:59Z"
})

// Grant flows through Layer 5 → Layer 1 → Layer 0 (stored)
// Agent Ether (Layer 3) can now query it for delegation decisions
```

---

## Legal/Agency Law Grounding

Authorization maps directly to **agency law** (*Restatement Third of Agency*):

| Agency Law Concept | Authorization Primitive |
|-------------------|------------------------|
| **Principal** | `principal` field (who grants) |
| **Agent** | `agent` field (who receives) |
| **Actual Authority** | `AuthorizationGrant` existence + validity |
| **Scope of Authority** | `scope` field (what actions) |
| **Limitations** | `constraints` field (budgets, restrictions) |
| **Duration** | `valid_from`, `valid_until` (temporal bounds) |
| **Revocation** | `revoked_at` field |

**Legal principle**: *An agent acts with actual authority when, at the time of taking action, the agent reasonably believes the principal wishes the agent so to act* (§2.01).

**Authorization provides proof** that principal granted authority, with scope, at a specific time.

---

## Examples

### Example 1: Deployment Authorization

```python
# Principal (Alice) grants deployment agent permission
grant = AuthorizationGrant(
    grant_id="auth:grant:abc123",
    principal="did:user:alice",
    agent="did:agent:deployment-bot",
    scope=["deploy-production", "rollback-production"],
    constraints={
        "budget_usd": 1000.00,
        "max_instances": 10,
        "allowed_regions": ["us-west-2", "eu-west-1"],
        "requires_approval_over": 500.00  # Dollar threshold
    },
    valid_from=datetime(2025, 12, 1),
    valid_until=datetime(2025, 12, 31, 23, 59, 59),
    delegation_depth=0,  # Cannot subdelegate
    granted_at=datetime(2025, 12, 1, 10, 0, 0),
    provenance_node="gg:node:xyz789"
)

# Later: Agent attempts deployment
deployment_task = Task(
    action="deploy-production",
    params={"region": "us-west-2", "instances": 5, "estimated_cost": 450.00}
)

# Agent Ether checks authorization
auth = query_authorization("did:agent:deployment-bot", "deploy-production")
✓ Grant exists
✓ Not expired (now < valid_until)
✓ Not revoked
✓ Action in scope ("deploy-production" in grant.scope)
✓ Constraints satisfied:
  - estimated_cost (450) < budget_usd (1000) ✓
  - instances (5) ≤ max_instances (10) ✓
  - region ("us-west-2") in allowed_regions ✓
  - No approval required (450 < 500 threshold) ✓

→ Deployment authorized, proceed
```

### Example 2: Budget Exhaustion

```python
# Same grant, but agent has consumed budget
grant.constraints["budget_used"] = 950.00  # Track spending

deployment_task = Task(
    action="deploy-production",
    params={"region": "us-west-2", "instances": 3, "estimated_cost": 200.00}
)

# Check constraints
budget_remaining = grant.constraints["budget_usd"] - grant.constraints["budget_used"]
# 1000 - 950 = 50

if deployment_task.estimated_cost > budget_remaining:
    return Error("Budget exhausted: $200 requested, $50 remaining")

→ Deployment blocked, escalate to principal
```

### Example 3: Revocation

```python
# Principal revokes authorization mid-way
revoke_authorization(grant_id="auth:grant:abc123")
# Sets grant.revoked_at = datetime.now()

# Agent attempts action
auth = query_authorization("did:agent:deployment-bot", "deploy-production")
if auth.revoked_at:
    return Error(f"Authorization revoked at {auth.revoked_at}")

→ All future actions blocked
→ Agent's capability (TAP) still exists, but permission removed
```

---

## Validation Rules

**When creating AuthorizationGrant**:

1. **Principal must exist** (valid DID)
2. **Agent must exist** (valid DID)
3. **Scope must be non-empty** (at least one action)
4. **Temporal bounds must be valid** (`valid_from < valid_until`)
5. **Constraints must be well-formed** (valid JSON, types correct)

**When checking authorization**:

1. **Grant must exist** for (agent, action) pair
2. **Must not be expired** (`now < valid_until`)
3. **Must not be revoked** (`revoked_at == None`)
4. **Action must be in scope** (`action in grant.scope`)
5. **Constraints must be satisfied** (all constraints pass)
6. **Delegation depth must not exceed limit** (if subdelegating)

**Constraint validation** (type-specific):

```python
def validate_constraints(task: Task, constraints: Dict) -> bool:
    for key, limit in constraints.items():
        if key == "budget_usd":
            if task.estimated_cost > limit:
                return False
        elif key == "max_api_calls":
            if task.api_calls > limit:
                return False
        elif key == "allowed_domains":
            if task.domain not in limit:
                return False
        # ... other constraint types
    return True
```

---

## Failure Modes & Mitigations

### Failure Mode 1: Overly Broad Grants

**Risk**: Principal grants authorization with scope="*" (all actions)

**Mitigation**:
- UI warns when scope > 5 actions
- Require explicit enumeration (no wildcards in v1)
- Audit trail shows broad grants for review

### Failure Mode 2: Forgotten Expiration

**Risk**: Grant valid_until = far future (effectively permanent)

**Mitigation**:
- UI defaults to 30-day expiration
- Warn when valid_until > 90 days
- Periodic review reminders

### Failure Mode 3: Constraint Bypass

**Risk**: Agent modifies task params to evade constraints

**Mitigation**:
- Constraint validation in Agent Ether (not agent-side)
- Provenance records original task params + constraint evaluation
- Tampering detectable in audit

### Failure Mode 4: Revocation Not Honored

**Risk**: Agent caches authorization, ignores revocation

**Mitigation**:
- Agent Ether always queries GenesisGraph (no caching)
- Revocation propagates immediately
- Failed revocation check = hard stop

---

## Relationship to Other Protocols

### TAP (Trust Assertion Protocol)

**TAP proves capability**:
```json
{ "type": "has-capability", "value": "deploy-production" }
```

**Authorization proves permission**:
```python
AuthorizationGrant(scope=["deploy-production"], ...)
```

**Both required**:
```python
if has_capability(agent, action) AND has_authorization(agent, action):
    proceed()
```

See: TRUST_ASSERTION_PROTOCOL.md (TAP vs Authorization section)

### Delegation (Hierarchical Agency)

**Authorization enables delegation**:
```python
# Alice grants deployment-bot permission
alice_grant = AuthorizationGrant(
    principal="did:user:alice",
    agent="did:agent:deployment-bot",
    delegation_depth=1  # Can subdelegate once
)

# Deployment-bot subdelegates to region-specific agent
subgrant = AuthorizationGrant(
    principal="did:agent:deployment-bot",  # Now principal
    agent="did:agent:us-west-deployer",
    scope=alice_grant.scope,  # Must narrow or equal
    delegation_depth=0,  # Decremented (no further delegation)
    derived_from=alice_grant.grant_id  # Provenance link
)
```

See: HIERARCHICAL_AGENCY_FRAMEWORK.md (delegation constraints)

### Intent Contracts

**IntentContract references AuthorizationGrant**:
```python
intent = IntentContract(
    goal="Deploy new feature",
    authorization_id="auth:grant:abc123",  # Links to grant
    ...
)
```

**Authorization validates intent execution**:
- Intent says "what" user wants
- Authorization says "may the agent do it"

See: INTENT_VERIFICATION_PROTOCOL.md

---

## Implementation Guidance

### Storage (GenesisGraph)

```python
# Create grant → store as graph edge
def create_authorization_grant(grant: AuthorizationGrant):
    edge = {
        "type": "grants-authorization",
        "from": grant.principal,
        "to": grant.agent,
        "metadata": {
            "grant_id": grant.grant_id,
            "scope": grant.scope,
            "constraints": grant.constraints,
            "valid_from": grant.valid_from.isoformat(),
            "valid_until": grant.valid_until.isoformat(),
            "revocable": grant.revocable,
            "delegation_depth": grant.delegation_depth,
        },
        "provenance": {
            "created_at": grant.granted_at.isoformat(),
            "created_by": grant.principal,
        }
    }
    genesis_graph.add_edge(edge)
    return grant.grant_id
```

### Query (Agent Ether)

```python
# Check if agent has authorization for action
def query_authorization(agent_id: str, action: str) -> Optional[AuthorizationGrant]:
    # Query GenesisGraph for edges: * --[grants-authorization]--> agent
    edges = genesis_graph.query(
        edge_type="grants-authorization",
        to_node=agent_id
    )

    for edge in edges:
        grant = AuthorizationGrant.from_edge(edge)

        # Check validity
        if grant.revoked_at:
            continue  # Skip revoked

        now = datetime.now()
        if now < grant.valid_from or now >= grant.valid_until:
            continue  # Skip expired/pending

        # Check scope
        if action in grant.scope:
            return grant

    return None  # No valid grant found
```

### Revocation

```python
def revoke_authorization(grant_id: str, principal: str):
    # Only principal who granted can revoke
    edge = genesis_graph.get_edge_by_grant_id(grant_id)

    if edge.metadata["from"] != principal:
        raise PermissionError("Only granting principal can revoke")

    # Mark as revoked (immutable append to graph)
    edge.metadata["revoked_at"] = datetime.now().isoformat()
    edge.metadata["revoked_by"] = principal

    genesis_graph.update_edge(edge)
```

---

## CLI Integration

```bash
# Grant authorization
$ tia auth grant \
    --agent did:agent:deployment-bot \
    --scope deploy-production,rollback-production \
    --budget 1000 \
    --expires 2025-12-31

Authorization granted: auth:grant:abc123

# List authorizations for agent
$ tia auth list --agent did:agent:deployment-bot

Grant ID: auth:grant:abc123
Agent: did:agent:deployment-bot
Scope: deploy-production, rollback-production
Constraints: budget_usd=1000, max_instances=10
Valid: 2025-12-01 to 2025-12-31
Status: ACTIVE

# Check if action authorized
$ tia auth check \
    --agent did:agent:deployment-bot \
    --action deploy-production

✓ Authorized (grant: auth:grant:abc123)
  Budget remaining: $550 / $1000

# Revoke authorization
$ tia auth revoke auth:grant:abc123

Authorization revoked at 2025-12-15T10:30:00Z
```

---

## See Also

- **TRUST_ASSERTION_PROTOCOL.md** — TAP vs Authorization distinction
- **HIERARCHICAL_AGENCY_FRAMEWORK.md** — Authority levels, delegation rules
- **INTENT_VERIFICATION_PROTOCOL.md** — Intent references authorization
- **SIL_GLOSSARY.md** — Authorization, AuthorizationGrant definitions
- **SIL_SAFETY_THRESHOLDS.md** — Safety constraints in authorization

---

**Status**: Specification complete (2025-12-12)
**Layer**: Cross-cutting (Layer 0 storage, Layer 1 semantics, Layer 3 enforcement, Layer 5 UI)
**Depends on**: TAP, GenesisGraph, Pantheon IR
**Enables**: Safe multi-agent delegation, legal defensibility, forensic accountability
