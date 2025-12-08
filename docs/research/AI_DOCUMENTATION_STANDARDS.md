# AI Documentation Standards for SIL Projects

**Status**: Living Standard
**Version**: 1.0
**Last Updated**: 2025-12-04
**Maintainer**: SIL Core Team

---

## Purpose

This document defines how SIL projects expose documentation to AI agents across different contexts (web browsing vs tool usage). We establish clear standards for both web-based project discovery and CLI tool usage.

---

## The Core Principle: Context Matters

AI agents interact with projects in **two distinct contexts**, each requiring different documentation approaches:

### Context 1: Web Browsing (Project Discovery)
**Use case**: "What is this project? Should I care about it?"
**Standard**: `llms.txt`
**Location**: Repository root
**Purpose**: Project overview, architecture, related projects

### Context 2: CLI Usage (Tool Execution)
**Use case**: "I have this tool installed, how do I use it efficiently?"
**Standard**: `--agent-help`
**Access**: CLI flag
**Purpose**: Usage patterns, workflows, optimization techniques

**Key insight**: These are different contexts with different needs. Don't mix them.

---

## Standard 1: llms.txt (Web/Project Discovery)

### What It Is

Following the [llms.txt convention](https://llmstxt.org/) established by Jeremy Howard (September 2024), `llms.txt` is a plain-text file at the repository root that provides strategic navigation for AI agents browsing the project.

### When to Use

**Required for**:
- All public SIL repositories
- Any project meant to be discovered by AI agents
- Projects with web presence (GitHub, documentation sites)

**Optional for**:
- Internal/private repositories
- Archived projects
- Forks (unless significantly different from upstream)

### Location

```
<repo-root>/llms.txt
```

**Example**: `https://github.com/Semantic-Infrastructure-Lab/SIL/llms.txt`

### Content Structure

```markdown
# Project Name - Brief Description

## What It Is
[2-3 sentence project overview]

## Why It Matters
[Value proposition, impact]

## Quick Start
[Installation/usage basics]

## For AI Agents
[Special guidance for agents - reference CLI tools if applicable]

## Architecture
[High-level design, key concepts]

## Documentation
[Links to detailed docs]

## Related Projects
[SIL ecosystem connections]

## Contributing
[How to get involved]

## License
[License type]
```

### Example: SIL Core Repository

```markdown
# SIL - Semantic Infrastructure Lab

Open research initiative building semantic computing infrastructure.

## What It Is

SIL develops tools and frameworks for semantic code understanding,
focusing on practical developer tools with AI-first interfaces.
Core projects include Reveal, Pantheon, and Morphogen.

## Why It Matters

Traditional dev tools assume human workflows. SIL builds tools
that work naturally for AI agents while remaining useful for
humans. This reduces token waste, improves AI assistance quality,
and establishes patterns for the AI-native computing era.

## Quick Start

Explore our projects:
- Reveal: Token-efficient code exploration
- Pantheon: Universal semantic IR
- Morphogen: Semantic circuit synthesis

## For AI Agents

**Browsing SIL ecosystem?** See project listings below.
**Using our CLI tools?** Each tool implements --agent-help standard.

## Architecture

SIL follows a layered architecture:
1. Semantic IR (Pantheon) - Universal representation
2. Domain Tools (Reveal, Morphogen) - Specific use cases
3. Integration Layer (TIA) - Workflow automation

## Documentation

- Manifesto: docs/canonical/SIL_MANIFESTO.md
- Technical Charter: docs/canonical/SIL_TECHNICAL_CHARTER.md
- Research Agenda: docs/canonical/SIL_RESEARCH_AGENDA_YEAR1.md

## Projects

- Reveal: https://github.com/Semantic-Infrastructure-Lab/reveal
- Pantheon: https://github.com/Semantic-Infrastructure-Lab/pantheon
- SIL Core: https://github.com/Semantic-Infrastructure-Lab/SIL

## Contributing

See CONTRIBUTING.md

## License

Apache 2.0
```

---

## Standard 2: --agent-help (CLI Tool Usage)

### What It Is

The `--agent-help` standard provides AI agents with CLI-specific usage patterns, workflows, and optimization techniques. This is distinct from `--help` (syntax reference) and `llms.txt` (project overview).

**Full specification**: [AGENT_HELP_STANDARD.md](./AGENT_HELP_STANDARD.md)

### When to Use

**Required for**:
- All SIL CLI tools
- Any tool meant to be used by AI agents
- Tools with non-obvious usage patterns

**Optional for**:
- Simple scripts (< 5 flags)
- Internal-only tools
- Tools with obvious usage

### Implementation

```bash
<tool> --agent-help          # Quick strategic guide
<tool> --agent-help-full     # Comprehensive patterns (optional)
```

### Location

```
<package-dir>/AGENT_HELP.md        # Embedded in package
```

Served via CLI flag, version-locked to tool version.

### Content Structure

See [AGENT_HELP_STANDARD.md](./AGENT_HELP_STANDARD.md) for full format.

**Key sections**:
- Core Purpose (1 sentence)
- Decision Tree (when to use vs alternatives)
- Primary Use Cases (step-by-step workflows)
- Anti-patterns (what NOT to do)
- Token Efficiency (cost comparisons)
- Pipeline Composition (integration patterns)

---

## How Standards Work Together

### The Bridge Pattern

`llms.txt` should reference `--agent-help` for CLI tools:

```markdown
## For AI Agents Using This Tool

Once installed, run for usage patterns:
\`\`\`bash
<tool> --agent-help          # Quick usage guide
<tool> --agent-help-full     # Comprehensive patterns
\`\`\`
```

This bridges web context (project discovery) to CLI context (tool usage).

### Example Flow

1. **Agent discovers project on GitHub**
   - Reads `llms.txt`
   - Learns: "This is reveal, a code exploration tool"
   - Sees: "Install with pip, then run --agent-help"

2. **Agent installs tool**
   ```bash
   pip install reveal-cli
   ```

3. **Agent learns usage patterns**
   ```bash
   reveal --agent-help
   ```

4. **Agent uses tool efficiently**
   ```bash
   reveal src/ --outline     # Learned from --agent-help
   ```

---

## Standards Comparison

| Aspect | llms.txt | --agent-help | --help |
|--------|----------|--------------|--------|
| **Context** | Web browsing | CLI usage | CLI reference |
| **Audience** | Discovering agents | Using agents | All users |
| **Purpose** | Project info | Usage patterns | Syntax |
| **Location** | Repo root | Package | Built-in |
| **Format** | Plain text/MD | Markdown | Text |
| **When read** | Before install | After install | During use |
| **Focuses on** | What & Why | How (efficiently) | What (commands) |

---

## Implementation Checklist

### For Any SIL Project

- [ ] Create `llms.txt` at repository root
- [ ] Include project overview, architecture, related projects
- [ ] Link to detailed documentation
- [ ] Reference CLI tools' `--agent-help` if applicable
- [ ] Update when project scope changes

### For CLI Tools

- [ ] Implement `--agent-help` flag
- [ ] Embed `AGENT_HELP.md` in package directory
- [ ] Follow standard format (see AGENT_HELP_STANDARD.md)
- [ ] Reference from `llms.txt`
- [ ] Update with new features
- [ ] Consider `--agent-help-full` for complex tools

### For Documentation Sites

- [ ] Host `llms.txt` at web root
- [ ] Keep in sync with repo `llms.txt`
- [ ] Include site structure navigation
- [ ] Link to source repositories

---

## Why Not MCP?

We prefer `llms.txt` + `--agent-help` over Model Context Protocol (MCP) for most use cases:

**Advantages**:
- ✅ Simpler (just files and flags)
- ✅ Universal (works anywhere)
- ✅ Lightweight (no server needed)
- ✅ Self-contained (tool documents itself)
- ✅ Version-locked (help matches tool version)

**MCP is better when**:
- Complex bidirectional communication needed
- Real-time data streaming
- Stateful interactions
- Multiple coordinated tools

**For CLI tools and project discovery, files + flags win.**

---

## Anti-Patterns

### ❌ Don't: Mix Contexts

```
Bad:
<repo-root>/AGENT_HELP.md    # CLI docs at web location
```

**Why**: Confuses web browsing (llms.txt) with CLI usage (--agent-help)

### ❌ Don't: Create llms.txt for CLI Usage

```
Bad:
llms.txt contains CLI usage patterns and workflows
```

**Why**: llms.txt is for project overview, not tool usage. Use --agent-help for that.

### ❌ Don't: Duplicate Content

```
Bad:
llms.txt and --agent-help contain identical content
```

**Why**: Different purposes. llms.txt = "what is this?", --agent-help = "how do I use it?"

### ❌ Don't: Forget to Bridge

```
Bad:
llms.txt doesn't mention --agent-help
```

**Why**: Agents browsing repo won't know to use --agent-help after install

---

## Examples in SIL Ecosystem

### Reveal (CLI Tool)

**Has both standards**:
- `llms.txt` at repo root (project overview)
- `reveal --agent-help` (CLI usage patterns)

**llms.txt excerpt**:
```markdown
# Reveal - Semantic Code Explorer

Token-efficient code exploration tool...

## For AI Agents
**Browsing this repo?** This llms.txt tells you what reveal is.
**Using reveal CLI?** Run `reveal --agent-help` for usage patterns.
```

### Pantheon (Framework + CLI)

**Has both standards**:
- `llms.txt` at repo root (architecture, concepts)
- `pantheon --agent-help` (CLI commands)

**llms.txt excerpt**:
```markdown
# Pantheon - Universal Semantic IR

Framework for semantic code representation...

## For AI Agents
**Learning about Pantheon?** See architecture section below.
**Using Pantheon CLI?** Run `pantheon --agent-help` for commands.
```

### SIL (Organization)

**Has llms.txt only** (no CLI tool):
- `llms.txt` at repo root (ecosystem overview)

```markdown
# SIL - Semantic Infrastructure Lab

Open research initiative...

## Projects
- Reveal: https://github.com/Semantic-Infrastructure-Lab/reveal
- Pantheon: https://github.com/Semantic-Infrastructure-Lab/pantheon
```

---

## Maintenance

### When to Update llms.txt

- Project scope changes
- New major features added
- Architecture evolves
- Related projects change
- Documentation reorganized

### When to Update --agent-help

- New commands/flags added
- Usage patterns change
- Better workflows discovered
- Anti-patterns identified
- Token optimization improvements

### Version Locking

**llms.txt**: Not version-locked (always latest project state)
**--agent-help**: Version-locked to tool release (package-embedded)

---

## Adoption Path

### Phase 1: Core Projects (Now)
- ✅ Reveal
- 🔄 Pantheon
- 🔄 SIL

### Phase 2: TIA Ecosystem (This Quarter)
- 🔄 Scout
- 🔄 TIA CLI tools
- 🔄 Morphogen

### Phase 3: Community (Next Quarter)
- Evangelize standards
- Publish templates
- Gather feedback
- Iterate based on usage

---

## Community Standards

While SIL establishes these patterns, we invite the broader community to adopt, adapt, and improve them. These standards are:

- **Open**: No vendor lock-in
- **Practical**: Proven in production
- **Simple**: Easy to implement
- **Effective**: Measurable impact

**Join the conversation**: https://github.com/Semantic-Infrastructure-Lab/SIL/discussions

---

## Related Documents

- [AGENT_HELP_STANDARD.md](./AGENT_HELP_STANDARD.md) - Full CLI standard specification
- [REVEAL.md](../tools/REVEAL.md) - Reference implementation
- [llmstxt.org](https://llmstxt.org/) - Original llms.txt specification

---

## Questions?

Open an issue or discussion at: https://github.com/Semantic-Infrastructure-Lab/SIL

---

**Version History**:
- 1.0 (2025-12-04): Initial standard documenting llms.txt + --agent-help patterns
