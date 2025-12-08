# SIL Core Principle #10: Tool Introspection & Quality Monitoring

**"Sharpen your chisel before working the wood. Monitor tool effectiveness before trusting results."**

**Rank**: #10 - **META-FEEDBACK PRINCIPLE**

---

## The Core Insight

Before using tools to do work, **verify the tools themselves are working effectively**. This is semantic system hygiene - analogous to "sharpen your chisel before woodworking" or "calibrate your instruments before measuring."

**The Pattern**:
```
Before using Beth → Check: Is Beth index healthy?
Before using reveal → Check: Does reveal work on target files?
Before using search → Check: Are search results relevant?
Before deploying agents → Check: Are their tools functioning?
```

**Why This Matters**:
- Bad tools produce bad work (garbage in → garbage out)
- Tool degradation is invisible without monitoring
- Early detection prevents cascading failures
- Feedback loops require working sensors

---

## The Problem: Invisible Tool Degradation

**Scenario 1: Beth Index Corruption**
```bash
# User: "Find deployment docs"
tia beth explore "deployment"
# Returns: 0 results

# Without monitoring, you assume:
❌ "No deployment docs exist" (wrong conclusion)

# With monitoring, you discover:
✅ "Beth index is stale/corrupted" (root cause)
```

**Scenario 2: Search Indexing Lag**
```bash
# User just created: docs/NEW_FEATURE.md
tia search all "NEW_FEATURE"
# Returns: 0 results

# Without monitoring:
❌ "File doesn't exist?" (confusion)

# With monitoring:
✅ "Search index hasn't rebuilt yet" (understanding)
```

**Scenario 3: Reveal Version Mismatch**
```bash
# CLAUDE.md has examples for reveal v0.15
# But system has reveal v0.9

# Without monitoring:
❌ Agent tries --check flag → command fails → confusion

# With monitoring:
✅ "reveal outdated, upgrade available" (actionable)
```

**The Core Problem**: Tool failures look like "no information exists" rather than "tool broken."

---

## The Solution: Systematic Tool Monitoring

### Level 1: Boot-Time Health Checks

**Already Implemented in `tia-boot`**:
```bash
## System Validation
✅ Tasks
✅ Search
✅ Domains
✅ AI
✅ Semantic
✅ Gemma
✅ Beth index healthy (14,459 files, 36,910 keywords)
✅ Beth
✅ Infrastructure
```

**What This Catches**:
- Beth index corruption
- Missing dependencies
- Service failures
- Configuration errors

**Pattern**: Every session starts with tool validation.

---

### Level 2: Pre-Task Tool Verification

**Before relying on a tool, verify it works for your specific use case.**

#### Example 1: Beth Effectiveness Check

```bash
# BEFORE doing research on "authentication patterns"
# First, verify Beth can find known-good docs:

tia beth explore "SIL core principles"
# Expected: Should return SIL_CORE_PRINCIPLES.md (this doc!)

# If returns 0 results → Beth broken, fix before continuing
# If returns expected docs → Beth working, proceed with confidence
```

#### Example 2: Reveal Version Check

```bash
# BEFORE relying on reveal features
reveal --version
# Shows: reveal 0.9.0

# Check against CLAUDE.md expectations
# CLAUDE.md expects: reveal v0.15+ (for --check flag)

# Decision:
# - Upgrade reveal, OR
# - Don't use --check flag (not available)
```

#### Example 3: Search Relevance Check

```bash
# BEFORE complex search task
# Test search quality with known query:

tia search all "tia-boot"
# Expected: Should find bin/tia-boot

# If no results → search index broken
# If wrong results → search needs tuning
# If correct results → proceed
```

**The Pattern**:
```
Known Query (Calibration) → Verify Expected Result → Proceed or Fix
```

---

### Level 3: Continuous Quality Monitoring

**Track tool effectiveness over time.**

#### Beth Health Metrics

```bash
# Regular health checks
tia beth health
# Reports:
# - Index size (files, keywords)
# - Last rebuild time
# - Coverage % (files indexed / files discovered)
# - Query success rate

# Example output:
Beth Health Report
==================
Index Size: 14,459 files, 36,910 keywords
Last Rebuild: 2 hours ago
Coverage: 98.7% (14,459 / 14,651 files)
Avg Query Time: 362ms
Success Rate: 87% (queries returning >0 results)

⚠️  Warning: 192 files not indexed (permission errors)
💡 Tip: Run `tia beth rebuild` to refresh
```

#### Search Quality Metrics

```bash
# Track search effectiveness
tia search metrics

# Reports:
# - Query patterns (most common searches)
# - Hit rate (% queries with results)
# - Result relevance (click-through on top results)
# - Index freshness (last update)

Search Metrics (Last 7 Days)
=============================
Total Queries: 342
Hit Rate: 94% (322/342 found results)
Avg Results: 8.2 per query
Index Freshness: 6 hours old

Top Queries:
  1. "tia-boot" (45 queries, 100% hit rate)
  2. "SIL" (38 queries, 97% hit rate)
  3. "reveal features" (22 queries, 91% hit rate)

⚠️  Zero-result queries (20):
  - "new_feature_xyz" (file not indexed yet)
  - "deployment automation" (poor term matching)
```

#### Reveal Quality Checks

```bash
# Verify reveal works on representative files
reveal --check projects/scout/lib/core.py

# Reports:
# - Parse success/failure
# - Structure extraction quality
# - Performance (time to parse)

Reveal Quality Check: projects/scout/lib/core.py
=================================================
✅ Parse: Success
✅ Structure: 12 classes, 45 functions extracted
✅ Performance: 127ms
⚠️  Note: 2 complex decorators skipped (unsupported syntax)
```

---

### Level 4: Automated Feedback Loops

**Tools monitor themselves and auto-correct.**

#### Auto-Rebuild Triggers

```python
# Beth auto-rebuilds when staleness detected
class BethMonitor:
    def check_health(self):
        if self.index_age > timedelta(hours=24):
            logger.warning("Beth index >24h old, triggering rebuild")
            self.rebuild_index()

        if self.coverage < 0.95:
            logger.warning(f"Beth coverage {self.coverage:.1%}, rebuilding")
            self.rebuild_index()
```

#### Search Index Auto-Update

```python
# Search watches file system, auto-indexes new files
class SearchMonitor:
    def on_file_created(self, path: Path):
        logger.info(f"New file detected: {path}, indexing...")
        self.index_file(path)

    def on_file_modified(self, path: Path):
        logger.info(f"File modified: {path}, re-indexing...")
        self.reindex_file(path)
```

#### Tool Version Alerts

```bash
# During boot, check for outdated tools
tia-boot
# Output includes:
⚠️  Update available: reveal 0.16.0 (you have 0.9.0)
    Update with: pip install --upgrade reveal-cli

⚠️  Update available: scout 2.1.0 (you have 1.8.0)
    Update with: cd projects/scout && git pull
```

---

## Real-World Workflows

### Workflow 1: Research Task with Tool Verification

```bash
# Task: Research "authentication patterns" across codebase

# STEP 0: Verify tools BEFORE starting
tia-boot  # Validates all tools
tia beth explore "SIL"  # Calibration check (known-good query)
# Expected: Returns SIL docs
# ✅ Beth working

# STEP 1: Now proceed with confidence
tia beth explore "authentication patterns"
# Returns: 12 results

# STEP 2: If unexpected results
# Before assuming "no auth docs exist"
# Check: Is Beth index fresh?
tia beth health
# Shows: Last rebuild 3 days ago, coverage 87%
# → Stale index! Rebuild and retry

tia beth rebuild
tia beth explore "authentication patterns"
# Returns: 24 results (was missing 12 docs!)
```

### Workflow 2: Code Exploration with Reveal Check

```bash
# Task: Understand structure of large Python project

# STEP 0: Verify reveal works
reveal --version
# v0.9.0

# Check: Does it work on a known file?
reveal bin/tia-boot
# ✅ Returns structure successfully

# STEP 1: Proceed to target
reveal projects/scout/lib/orchestrator.py --outline
# Returns clear hierarchy

# STEP 2: Extract specific function
reveal projects/scout/lib/orchestrator.py run_campaign
# ✅ Returns function implementation
```

### Workflow 3: Deployment with Tool Checks

```bash
# Task: Deploy new SIL documentation to staging

# STEP 0: Verify deployment tools
tia secrets get github:gh_session  # ✅ Auth works
gh auth status  # ✅ GitHub CLI authenticated
tia git health  # ✅ Git repo healthy

# STEP 1: Proceed with deployment
cd projects/SIL
tia git make-clean  # Clean up repo
git push origin staging  # Deploy

# STEP 2: Verify deployment
curl https://staging.sil.dev/docs/  # ✅ Live
```

---

## The Feedback Loop Structure

**This is a meta-feedback loop** - monitoring the monitors:

```
┌─────────────────────────────────────────────────┐
│ Primary Feedback Loop (Intent → Execution)      │
│                                                  │
│  User Intent → Tool Usage → Results → Learning  │
│                    ↑                             │
│                    │                             │
│                    │ Are tools working?         │
│                    │                             │
└────────────────────┼─────────────────────────────┘
                     │
                     ↓
┌─────────────────────────────────────────────────┐
│ Meta-Feedback Loop (Tool Quality)               │
│                                                  │
│  Boot Checks → Health Monitoring → Auto-Repair  │
│       ↓              ↓                 ↓         │
│   ✅ Beth        ⚠️  Coverage      🔧 Rebuild   │
│   ✅ Search      ⚠️  Staleness     🔧 Reindex   │
│   ✅ Reveal      ⚠️  Version       🔧 Upgrade   │
└─────────────────────────────────────────────────┘
```

**Connection to SEMANTIC_FEEDBACK_LOOPS.md**:
- Primary loop: Measure intent-execution alignment
- Meta loop: Measure tool-effectiveness alignment
- Both required: Can't have good execution with broken tools

**Connection to SEMANTIC_OBSERVABILITY.md**:
- Observability instruments the primary loop (user satisfaction)
- Tool monitoring instruments the meta loop (tool health)
- Nested observability: Observe the observers

---

## Application to Agent Systems

**Critical for autonomous agents** - agents can't self-correct with broken tools.

### Scout Agent Tool Checks

```python
# Before Scout starts research campaign
class ScoutPreflightCheck:
    def verify_tools(self):
        checks = [
            self.check_llm_api(),      # Can reach Groq/Anthropic?
            self.check_search(),        # Search index working?
            self.check_beth(),          # Beth healthy?
            self.check_file_access(),   # Can read/write files?
        ]

        if not all(checks):
            raise ToolFailureError("Preflight checks failed, aborting")

        logger.info("✅ All tools verified, proceeding with campaign")
```

### Agent-Ether Tool Monitoring

```python
# Agent-Ether monitors tool health during multi-agent orchestration
class AgentEtherMonitor:
    def before_agent_spawn(self, agent_config):
        # Verify agent has working tools
        for tool in agent_config.required_tools:
            if not self.verify_tool(tool):
                logger.error(f"Tool {tool} not working, cannot spawn agent")
                return False

        return True

    def verify_tool(self, tool_name: str) -> bool:
        """Run calibration check on tool"""
        if tool_name == "beth":
            # Known-good query
            results = beth.search("SIL")
            return len(results) > 0

        elif tool_name == "reveal":
            # Can parse a simple file?
            test_file = Path("bin/tia-boot")
            return reveal.extract_structure(test_file) is not None

        # ... other tools
```

---

## Measuring Tool Quality

### Quantitative Metrics

**Beth Health**:
- Index coverage: >95% (files indexed / files discovered)
- Query success rate: >85% (queries with results)
- Index freshness: <24 hours old
- Avg query time: <500ms

**Search Health**:
- Hit rate: >90% (queries finding results)
- Index lag: <1 hour (time from file change to indexed)
- Result relevance: >80% (user clicks top 3 results)

**Reveal Health**:
- Parse success: >98% (files successfully parsed)
- Performance: <200ms for typical files
- Version currency: Within 2 minor versions of latest

### Qualitative Indicators

**Green Flags** (tools working well):
- ✅ Beth consistently finds expected docs
- ✅ Search returns relevant results quickly
- ✅ Reveal parses complex files without errors
- ✅ Boot checks pass every session
- ✅ Zero tool-related support questions

**Red Flags** (tool degradation):
- ❌ Beth returning 0 results for known topics
- ❌ Search missing recently created files
- ❌ Reveal failing on valid Python files
- ❌ Boot checks showing warnings
- ❌ Users complaining "can't find anything"

---

## Implementation Checklist

### For TIA System

- [x] **Boot-time health checks** (`tia-boot` validation section)
- [ ] **Beth health command** (`tia beth health`)
- [ ] **Search metrics** (`tia search metrics`)
- [ ] **Reveal version check** (auto-notify on outdated)
- [ ] **Auto-rebuild triggers** (Beth/search staleness detection)
- [ ] **Tool calibration tests** (known-good query suite)

### For Agents (Scout, Agent-Ether)

- [ ] **Preflight checks** (verify tools before starting work)
- [ ] **Mid-flight monitoring** (detect tool failures during execution)
- [ ] **Graceful degradation** (fallback when tools fail)
- [ ] **Tool failure reporting** (alert human when tools broken)

### For Documentation

- [ ] **Add to SIL_CORE_PRINCIPLES.md** (Principle #10)
- [ ] **Update CLAUDE.md template** (emphasize tool verification)
- [ ] **Create tool health guide** (how to monitor each tool)
- [ ] **Document calibration tests** (known-good queries for each tool)

---

## Connection to Existing SIL Principles

### Synergy with Other Principles

**#1: Progressive Disclosure**:
- Tool monitoring uses progressive disclosure (boot checks → health reports → detailed diagnostics)

**#2: Composability First**:
- Each tool monitors itself independently
- Monitoring tools are composable (beth health + search metrics + reveal check)

**#8: Human-in-the-Loop**:
- Tool degradation alerts require human attention
- Auto-repair for low-risk (rebuild index), human approval for high-risk (upgrade tools)

**#9: Examples as Multi-Shot Reasoning Anchors**:
- Calibration tests ARE examples (known-good queries)
- Agents learn "this is what good results look like"

### Extends Existing Work

**SEMANTIC_FEEDBACK_LOOPS.md**:
- Primary feedback: User intent → execution → measurement
- **Meta feedback**: Tool health → monitoring → auto-repair
- Nested loops: Can't measure execution quality with broken tools

**SEMANTIC_OBSERVABILITY.md**:
- Observability framework measures intent-execution alignment
- **Tool monitoring measures tool-health alignment**
- Both required for semantic system reliability

---

## The "Sharpen Your Chisel" Analogy

**Woodworking**:
- Dull chisel → poor cuts, wasted effort, frustration
- Sharp chisel → clean cuts, efficient work, quality results
- **Master carpenters sharpen tools BEFORE starting work**

**Semantic Systems**:
- Broken tools → wrong results, wasted tokens, confusion
- Working tools → accurate results, efficient search, confidence
- **Master agents verify tools BEFORE starting research**

**The Discipline**:
```
Apprentice: Starts work immediately, struggles with dull tools
Master: Sharpens tools first, works efficiently

Junior Agent: Uses Beth blindly, gets 0 results, assumes "no docs exist"
Senior Agent: Checks Beth health, discovers stale index, rebuilds, finds 24 docs
```

---

## Key Takeaways

1. **Tool degradation is invisible** without monitoring
2. **Boot-time health checks** catch most failures early
3. **Calibration tests** (known-good queries) verify tool effectiveness
4. **Continuous monitoring** catches gradual degradation
5. **Auto-repair loops** reduce human intervention
6. **Agents MUST verify tools** before autonomous work
7. **Meta-feedback loop** monitors the monitors

**The Pattern**:
```bash
# Before every significant task:
1. tia-boot                        # Verify system health
2. <tool> <calibration_test>       # Verify specific tool works
3. Proceed with confidence         # Tools are sharp, work efficiently
```

**Remember**:
- Garbage tools → garbage results
- Sharp tools → quality work
- **Always sharpen your chisel before working the wood**

---

## Next Steps

### Immediate (This Session)
1. Review this principle with user
2. Decide if this becomes SIL Core Principle #10
3. Create implementation plan (commands, code, docs)

### Short-Term (Next Week)
1. Implement `tia beth health` command
2. Implement `tia search metrics` command
3. Add calibration test suite (known-good queries)
4. Update CLAUDE.md with tool verification patterns

### Medium-Term (Next Month)
1. Add auto-rebuild triggers (Beth/search staleness detection)
2. Implement Scout preflight checks
3. Create tool health dashboard
4. Document tool monitoring best practices

### Long-Term (Next Quarter)
1. Full automated tool monitoring infrastructure
2. Predictive tool degradation detection
3. Self-healing semantic systems
4. Tool quality as first-class observability metric

---

**Status**: Draft - awaiting review
**Path**: `/home/scottsen/src/tia/sessions/burning-moon-1206/TOOL_QUALITY_MONITORING_PRINCIPLE.md`
**Next**: Review → Edit → Integrate into SIL_CORE_PRINCIPLES.md
