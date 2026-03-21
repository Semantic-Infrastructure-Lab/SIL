---
title: "Reveal: A Semantic Query Layer for Code, Infrastructure, and Docs"
subtitle: "Progressive disclosure, URI-based queries, and 23 adapters — what it actually does and why it matters"
author: "Scott Senkeresty"
date: "2026-03-20"
type: "article"
status: "published"
audience: "developers"
topics: ["reveal", "progressive-disclosure", "token-efficiency", "code-exploration", "semantic-stack", "uri-adapters", "call-graph", "ai-agents", "mcp"]
related_projects: ["reveal", "beth", "sil"]
related_docs:
  - "PROGRESSIVE_DISCLOSURE_GUIDE.md"
  - "REVEAL_BETH_PROGRESSIVE_KNOWLEDGE_SYSTEM.md"
  - "../systems/reveal.md"
canonical_url: "https://semanticinfrastructurelab.org/articles/reveal-introduction"
reading_time: "15 minutes"
beth_topics: [reveal, progressive-disclosure, token-efficiency, uri-adapters, call-graph, pack, code-review, agent-help, mcp-server, sil]
session_provenance: "yemiti-0320"
version: "v0.66.0"
---

# Reveal: A Semantic Query Layer for Code, Infrastructure, and Docs

**Or: one consistent syntax for asking questions about everything in your project**

---

You know the moment. Claude needs to understand your auth module, so it reads `auth.py`. Then it reads `config.py` because auth imports it. Then `models.py`, `utils.py`, and three more files because who knows what's actually needed. By the time it gets to your question, it's burned 15,000 tokens just navigating the codebase.

This is the default. It doesn't have to be.

**Reveal** solves this with progressive disclosure — a structured query layer that gives you (and AI agents) exactly what you need without reading everything. But that's just where it starts. By the time we're done here, you'll have seen call-graph analysis, token-budgeted codebase snapshots, automated PR review, unified health checks spanning code and infrastructure, and documentation treated as a queryable graph.

Install it now if you want to follow along:

```bash
pip install reveal-cli
```

---

## Start Here: The Three-Level Drill-Down

Most tools give you two options: a directory listing (too little) or full file contents (too much). Reveal enforces three levels:

```bash
# Level 1: What's in this directory?
$ reveal src/
📁 src/ (12 files)
├── auth.py (342 lines, Python)
├── config.py (189 lines, Python)
├── models/
│   ├── user.py (156 lines, Python)
│   └── post.py (203 lines, Python)
└── ...
```

~100 tokens. You now know what exists. You don't need to read a single file.

```bash
# Level 2: What's in this file?
$ reveal src/auth.py
auth.py (342 lines, Python)
├── Imports (6)
├── Classes (1)
│   └── AuthManager (lines 18-287)
└── Functions (8)
    ├── validate_token (lines 291-318, complexity: 7)
    ├── refresh_token (lines 320-341, complexity: 4)
    └── ...
```

~250 tokens. You now know what functions exist, roughly how complex they are, and where they live. You still haven't read the file.

```bash
# Level 3: What does this function do?
$ reveal src/auth.py validate_token
src/auth.py:291-318
def validate_token(token: str, *, require_fresh: bool = False) -> AuthClaims:
    """Validate a JWT token and return decoded claims."""
    try:
        claims = jwt.decode(token, settings.SECRET_KEY, algorithms=["HS256"])
        if require_fresh and not claims.get("fresh"):
            raise TokenNotFreshError("Token is not fresh")
        return AuthClaims(**claims)
    except jwt.ExpiredSignatureError:
        raise TokenExpiredError(f"Token expired at {claims.get('exp')}")
    except jwt.InvalidTokenError as e:
        raise InvalidTokenError(str(e))
```

~150 tokens. Exact implementation, nothing else.

**Total: ~500 tokens.** Traditional approach (read all three relevant files): ~8,500 tokens. The 10-150x token reduction claim isn't marketing — it's structurally guaranteed by the architecture. Structure output is always smaller than content output.

But this is the easy part. The three-level drill-down is just the default interaction model. Here's what actually makes Reveal interesting.

---

## The URI Architecture: Everything Is a Resource

Most tools expose features as subcommands: `git log`, `git blame`, `docker ps`. You learn each command separately. Reveal takes a different approach — it exposes *resources* as URIs with query parameters.

```bash
reveal ast://src/?complexity>10&sort=-complexity
reveal calls://src/?target=validate_token&depth=3
reveal ssl://api.example.com
reveal mysql://prod/?type=replication
reveal markdown://docs/?aggregate=type
```

Same syntax. Same query operators (`=`, `~=`, `>`, `!`, `..`, `*`). Same output format. Whether you're querying code complexity, call graphs, SSL certificates, database replication status, or documentation metadata — the mental model is identical.

This isn't an aesthetic choice. It's what makes everything composable. The output of `nginx://` feeds `ssl://`. The output of `diff://` feeds `ast://`. Infrastructure becomes queryable data that pipes into the next analysis.

23 adapters ship today. Let's look at the ones you'll actually use.

---

## Scenario 1: "What Calls This Function?"

You're about to refactor `validate_token`. Before you touch it, you want to know: what else in this codebase depends on it?

With traditional tools, you'd run `grep -r "validate_token" src/` and manually parse the results to figure out what's a call versus a comment versus a string literal. In a large codebase, this quickly becomes noise.

```bash
# Who calls validate_token — anywhere in the project?
reveal 'calls://src/?target=validate_token'

# Output:
# validate_token — called by 6 functions across 4 files
#   src/middleware/auth.py:42  check_request_auth
#   src/api/routes.py:118      require_login (via decorator)
#   src/api/routes.py:203      require_fresh_token
#   src/tests/test_auth.py:67  test_expired_token
#   src/tests/test_auth.py:89  test_invalid_token
#   src/tests/test_auth.py:134 test_fresh_requirement
```

That's the immediate callers. But what if you want the full impact radius — callers of callers?

```bash
# Two levels deep — what calls the things that call validate_token?
reveal 'calls://src/?target=validate_token&depth=3'
```

This is the kind of analysis that normally requires an IDE with a language server running. Here it's a CLI query on any codebase, zero configuration.

```bash
# What does validate_token call? (dependency surface)
reveal 'calls://src/?callees=validate_token'

# Most architecturally load-bearing functions in the whole project
# (not "who calls this" but "what is called by the most things")
reveal 'calls://src/?rank=callers&top=20'

# Rough dead code detection — functions with no callers in the index
reveal 'calls://src/?uncalled&type=function'

# Visual call graph
reveal 'calls://src/?target=validate_token&format=dot' | dot -Tsvg > callgraph.svg
```

The `?rank=callers` metric is the one I find most useful. It surfaces architecturally load-bearing functions before you know to look for them — the functions everything depends on, not just the ones you happen to be thinking about.

---

## Scenario 2: "Give the AI Exactly the Right Codebase Context"

Here's a problem that comes up constantly when working with AI agents: how do you give Claude useful codebase context without burning its entire context window on boilerplate?

The naive approach is to read every file. The thoughtful approach is `reveal pack`:

```bash
# Fit the most important code in 8000 tokens
reveal pack src/ --budget 8000
```

What this does: ranks files by complexity and recency, fills entry points first, then priority files, stops at the budget. The agent gets the right code in the right order at the right size.

But the really useful version is the PR-aware snapshot:

```bash
# Changed files first, then fill with key dependencies
reveal pack src/ --since main --budget 8000
```

`--since main` boosts every file changed in the current branch to priority tier 0 — above entry points, above everything. The remaining budget fills with the most important context. When you hand this to an AI agent for a PR review, it leads with what actually changed, not what's biggest.

This is built for agents, not retrofitted. The token budget is a first-class parameter. The problem it solves — agents exhausting their context on stub `__init__.py` files before reaching the actual logic — is real and constant.

---

## Scenario 3: "Did This PR Make Anything Harder to Maintain?"

Most CI checks catch bugs. Very few catch *architectural decay* — the gradual accumulation of complexity that makes a codebase harder to work with over time.

```bash
# Full PR review: structural diff + quality violations + hotspots + complexity delta
reveal review main..HEAD
```

Under the hood, `reveal review` composes four things:
1. **Structural diff** — what functions and classes were added, removed, or modified
2. **Quality checks** — 69 rules across bugs, security, complexity, and more
3. **Hotspot detection** — which files got worse
4. **Complexity delta** — before/after complexity on every changed function

That last one is the novel part. Every modified function carries `complexity_before`, `complexity_after`, and `complexity_delta` in the output. You can now ask: "did this PR make anything meaningfully more complex?"

```bash
# Find functions that got more complex in this PR
reveal diff://git://main/.:git://HEAD/. --format json | \
  jq '.diff.functions[] | select(.complexity_delta > 5)'

# CI gate: exit 0 clean, exit 1 warnings, exit 2 errors
reveal review main..HEAD || exit 1
```

Drop this in a PR check. The baseline is the same whether a human or agent runs it. Complexity growth becomes a trackable metric, not something you only notice six months later when the codebase feels slow to work with.

---

## Scenario 4: "One Command for Code, Certs, DNS, and Database"

Most teams have three or four separate tools for infrastructure health: something for code quality, `openssl s_client` for certificates, a DNS checker, a MySQL monitoring query. None of them share output formats. None of them compose.

```bash
# Check everything at once
reveal health ./src ssl://api.example.com domain://example.com mysql://prod
```

One command. Code quality violations + SSL certificate expiry and chain validation + DNS health + MySQL replication status, all under unified exit codes and JSON output.

```bash
# Check just code + SSL together
reveal health ./src ssl://api.example.com

# JSON for monitoring pipelines
reveal health ./src --format json | jq '.overall_exit'

# CI gate
reveal health ./src && deploy.sh || alert_oncall.sh
```

The value isn't any individual check — it's that you stop switching tools. The nginx adapter is particularly useful here: `reveal nginx://yourdomain.com` parses your nginx vhost config and surfaces misconfigurations before they become incidents. Want to check every domain in your nginx config for SSL expiry?

```bash
reveal nginx.conf --extract domains | sed 's/^/ssl:\/\//' | reveal --stdin --check
```

The output of one adapter feeds the next. This is the composability the URI architecture enables.

---

## Scenario 5: "Our Docs Are a Graph, Not a Pile of Files"

If your project has significant documentation — architecture docs, guides, references, changelogs — `markdown://` turns it into a queryable graph.

```bash
# What document types exist across the knowledge base?
reveal 'markdown://docs/?aggregate=type'
# → guide: 23, reference: 18, architecture: 7, procedure: 12 ...

# Find all docs tagged with a topic
reveal 'markdown://docs/?beth_topics~=authentication'

# Which docs link to auth.md? (bidirectional link graph)
reveal 'markdown://docs/?link-graph' | grep 'auth.md'

# Find orphaned docs — nothing links to them
reveal 'markdown://docs/?link-graph' | grep 'backlinks: 0'

# Full-text search combined with structured metadata filtering
reveal 'markdown://docs/?body-contains=retry&type=procedure'
```

Bidirectional link graphs. Taxonomy frequency tables. Full-text search combined with frontmatter filtering. This is knowledge graph functionality in a code tool. For projects where the docs are first-class (not an afterthought), this changes how you maintain them.

---

## The Architecture Behind the Composability

These aren't independent features bolted together. They share a common architecture that makes them composable:

| Design choice | What it enables |
|---|---|
| URI-as-language | Same query syntax across all resources — code, infra, data, docs |
| Universal operators (`=`, `~=`, `>`, `!`, `..`, `*`) | Filter expressions transfer across adapters without learning new syntax |
| `--stdin` piping | Output of one adapter is input to the next |
| Structured JSON output | Every query composable with `jq`, scripts, and AI agents |
| Exit code discipline (0/1/2) | Every check is a valid CI gate with no configuration |
| Token budgeting | First-class LLM context constraint, not a workaround |
| `reveal://` self-reference | The tool introspects itself using its own syntax |

The self-reference property is worth dwelling on. `reveal help://schemas/ast` gives you a machine-readable JSON schema for the AST adapter. `reveal help://adapters` lists every adapter with syntax and examples. `reveal --agent-help` generates a condensed decision tree for AI agents encountering the tool for the first time.

This means agents can safely discover what Reveal offers and how to use it without external documentation. The tool documents itself. When you're building AI workflows, adoption friction drops to near zero.

---

## The MCP Server: Agents Get All of This Natively

```bash
pip install reveal-mcp
```

`reveal-mcp` exposes all of Reveal's capabilities as MCP tools for Claude Code, Cursor, and Windsurf. Five tools: `reveal_structure`, `reveal_element`, `reveal_query`, `reveal_pack`, `reveal_check`. Agents get progressive disclosure, call-graph analysis, and quality checks without subprocess overhead.

This is how the architecture of progressive disclosure becomes the default behavior for AI agents working on your codebase — not something they have to be instructed to do, but something built into how they access code.

---

## What Nobody Else Does

After using Reveal daily for months, here are the capabilities I haven't found elsewhere in a CLI tool:

**`?rank=callers`** — coupling metrics from the command line. Not "who calls this specific function" but "what functions in this project are called by the most other things." Graph analysis, not search.

**`reveal health` spanning code + certs + DB + DNS** — a genuine category collapse. One command, one JSON blob, one exit code, four different infrastructure domains.

**`complexity_delta` on every changed function** — `diff://` carries `complexity_before`, `complexity_after`, and `complexity_delta` for every modified function. "Did this PR make anything harder to maintain?" is now a scriptable CI check.

**`reveal pack --since <branch> --budget N`** — PR-aware codebase snapshots that boost changed files to priority tier 0. Built for AI agents; the token budget is a first-class parameter.

**`?depth=N` transitive call graphs** — callers-of-callers up to 5 levels deep. Impact radius before a refactor, not just immediate callers.

**`?decorator=*cache*` wildcard matching** — finds all functions with `@lru_cache`, `@cached_property`, `@cache`, or any decorator matching the pattern, without knowing exact names.

**`markdown://docs/?link-graph`** — bidirectional documentation link analysis with orphan detection. Rare in any tool category.

**`claude://sessions/` session archaeology** — AI work history as queryable structured data. Find every file a session touched, search across sessions for references to a function.

**Self-describing infrastructure** — `help://schemas/<adapter>` returns machine-readable JSON schemas for every adapter. Agents can discover capabilities programmatically without external documentation.

---

## The 23 Adapters

| Domain | Adapters |
|--------|----------|
| Code semantics | `ast://`, `calls://`, `imports://`, `diff://`, `python://` |
| Data systems | `mysql://`, `sqlite://`, `json://`, `xlsx://` |
| Infrastructure | `ssl://`, `nginx://`, `domain://`, `cpanel://`, `autossl://`, `letsencrypt://`, `env://` |
| Documents | `markdown://`, `stats://`, `git://` |
| Meta / self-referential | `help://`, `reveal://`, `claude://` |

190+ languages: 37 built-in analyzers plus 165 additional via Tree-sitter fallback. 69 quality rules across 14 categories. Zero configuration required.

---

## The Day-to-Day Patterns You'll Actually Use

The scenarios above are the highlights. In practice, these are the patterns you'll run dozens of times a day:

```bash
# Orient on an unfamiliar codebase
reveal .                               # directory structure
reveal src/                            # drill into source
reveal src/main.py                     # file outline
reveal src/main.py create_app          # extract one function

# Find things without reading files
reveal 'ast://src/?name=*auth*'        # anything with "auth" in the name
reveal 'ast://src/?complexity>10'      # functions that need attention
reveal 'ast://src/?decorator=*cache*'  # cached functions

# Understand architectural coupling
reveal 'calls://src/?rank=callers&top=20'      # what holds everything together
reveal 'calls://src/?target=fn_name'           # impact radius before refactoring
reveal 'imports://src/?circular'               # circular dependency chains

# Orient an AI agent
reveal overview .                      # codebase dashboard in one output
reveal deps .                          # dependency health
reveal hotspots src/                   # worst files by combined metrics
reveal pack src/ --since main --budget 8000  # PR context for agent

# Infrastructure health
reveal ssl://yourdomain.com            # certificate status
reveal nginx://yourdomain.com          # vhost config analysis
reveal health ./src ssl://api.example.com   # combined check

# Before/after a PR
reveal review main..HEAD               # full review with complexity delta
reveal review main..HEAD --select B,S  # fast: bugs + security only
```

---

## Try It

```bash
pip install reveal-cli
cd ~/your-project
reveal .
```

Pick any file that looks interesting. Run `reveal file.py`. See the outline in ~200 tokens instead of reading 5,000.

Then extract one function: `reveal file.py function_name`.

Then ask a structural question: `reveal 'ast://src/?complexity>10'`.

Then, if you have multiple services or a complex codebase, run `reveal overview .` and `reveal deps .`.

Then — and this is where it gets interesting — try `reveal 'calls://src/?rank=callers&top=20'` and see what your codebase actually depends on.

---

## Why This Exists

Reveal is the first production system from the **Semantic Infrastructure Lab** — a research project building the foundations for AI-native tooling. The core thesis: AI agents need infrastructure that meets them where they are, not human tools retrofitted for automated use.

Progressive disclosure is one piece of that. The same pattern that makes Reveal efficient for code will extend to semantic graphs, provenance chains, and multi-agent reasoning systems. But that's the longer story. This is the part that ships today, works immediately on your codebase, and makes your AI agent workflows measurably better.

**GitHub:** [Semantic-Infrastructure-Lab/reveal](https://github.com/Semantic-Infrastructure-Lab/reveal)
**PyPI:** [reveal-cli](https://pypi.org/project/reveal-cli/)
**MCP Server:** `pip install reveal-mcp`

---

*Reveal v0.66.0 — 6,861 tests, 23 URI adapters, 69 quality rules, 190+ languages. MIT license.*
