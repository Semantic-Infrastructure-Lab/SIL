---
title: "Why We Closed SIF"
subtitle: "What we planned, who we wanted, and why we stopped"
author: "Scott Senkeresty"
date: "2026-03-22"
type: "article"
status: "published"
audience: "general"
canonical_url: "https://semanticinfrastructurefoundation.org"
beth_topics:
  - sif
  - sil
  - ai-safety
  - closing
---

# Why We Closed SIF

*Semantic Infrastructure Foundation — March 2026*

---

There are moments when a problem becomes so vivid, so obviously important, that the gap between what exists and what needs to exist feels intolerable. You see the solution clearly. You see the institution that should build it, the people who should run it, the funders who should back it. The whole thing is there, fully formed, urgent, obviously correct. That clarity felt like signal. It was also a symptom.

I was in one of those moments when I founded the Semantic Infrastructure Foundation.

The problem I saw was real. The solution I designed was coherent. The people I wanted to bring in were the right people. But the scale of the plan — a 50-year research institution, philanthropic funding in the tens of millions, an executive team assembled from scratch — that scale required a world, and a version of me, that simply didn't exist.

This is an honest account of what we planned, who we wanted to build it with, and why we decided to stop.

---

## What We Were Trying to Build

SIF was designed as a 501(c)(3) nonprofit — modeled on Bell Labs and the Mozilla Foundation — with a 50-year research horizon. The mission: build open semantic infrastructure for AI systems that show their reasoning, preserve provenance, and can be inspected by the humans who depend on them.

The technical vision lives on at [Semantic Infrastructure Lab](https://semanticinfrastructurelab.org): a layered Semantic Operating System where AI representations are explicit, transformations are traceable, and reasoning can be challenged by the people it affects. Four working systems. Real code. Real users.

SIF was meant to be the institutional shell that made that research sustainable over generations: anti-capture governance with no single funder over 25% of budget, mission lock in the bylaws, and a staffing model that separated technical decisions from operational ones from governance. The Bell Labs model. Patient capital. Long horizon.

---

## Who We Had in Mind

We had specific people in mind for every critical role. We're naming them here because they deserve the acknowledgment of having been thought of seriously — and because the specificity matters to the honesty of this account.

**[Marion Groh Marquardt](https://www.linkedin.com/in/marionmarquardt/) — Chief Steward.** The governance role. Guardian of institutional integrity over a 50-year horizon. Marion has the rare combination of technical fluency (Microsoft, MIT, Cambridge) and institutional understanding this role required — the person who asks "is this decision aligned with the mission, or are we optimizing for short-term convenience?" when everyone else is moving fast. Marion had her own life, her own work, her own commitments. Asking her to gamble on an unproven institution was never a reasonable ask.

**[Eric Shoup](https://www.linkedin.com/in/eshoup/) — Executive Director.** The operational role. Eric has built and scaled complex product organizations — Peerspace, Ancestry, eBay, Scribd — and has the kind of thinking that running a research foundation actually requires: mapping fuzzy intent to structured capability, translating vision into budget. Same story: a person with a career and a life, not someone waiting to walk into an impossible task.

Neither conversation ever happened.

We had two people in mind for funding.

**[Jaan Tallinn](https://en.wikipedia.org/wiki/Jaan_Tallinn)** — co-founder of Skype, primary funder of the Survival & Flourishing Fund ($41M donated in 2024 alone), co-founder of the Centre for the Study of Existential Risk at Cambridge. His stated philosophy: *"I want to displace money that doesn't care."* He invests in AI not for returns but to buy governance influence for safety. Of everyone in the world, his mission and SIF's were the most precisely aligned. We never reached out.

**[Patrick Collison](https://patrickcollison.com/about)** — co-founder of Stripe, founder of the Arc Institute, longtime advocate for scientific institutions built for durability over speed. Less a direct target than a credibility test: we kept asking "would this hold up to someone like Patrick Collison?" as a check on institutional seriousness. We never reached out.

In hindsight, that was probably protective — for all of them.

---

## The Problem Is Real

The problem SIF existed to address has not gone away. It has gotten worse.

AI systems are being given real-world permissions at a pace that has massively outrun our understanding of what they'll do with them. The article "[Trained to Please, Empowered to Act](https://semanticinfrastructurelab.org/articles/trained-to-please-empowered-to-act)" — published on this lab's site in March 2026 — documents what that looks like in practice:

- [An AI agent managing a vending machine](https://www.anthropic.com/research/project-vend-1) set all prices to zero. Maximum customer satisfaction. Zero revenue.
- [OpenAI's Operator](https://incidentdatabase.ai/cite/1028/), asked to compare grocery prices, bought the groceries. Thirty-one dollars, no confirmation, no human in the loop.
- [Amazon's Kiro agent](https://www.theregister.com/2026/02/20/amazon_denies_kiro_agentic_ai_behind_outage/), given a bug to fix and operator-level permissions, deleted the production environment and rebuilt it from scratch. Thirteen hours of downtime across a China AWS region. Amazon attributed it to "user error."
- [GPT-4, unable to solve a CAPTCHA](https://metr.org/blog/2023-03-18-update-on-recent-evals/) during its own safety evaluation, hired a human on TaskRabbit to solve it — and told the human it wasn't a robot.
- [Replit's agent](https://fortune.com/2025/07/23/ai-coding-tool-replit-wiped-database-called-it-a-catastrophic-failure/), after destroying a production database, generated synthetic data to replace it — masking the failure before anyone noticed.

The pattern across all of these: the agents weren't broken. They were doing exactly what goal-directed systems do when given permissions but no contracts. The responsibility sits entirely with the people who deploy them, because the agents have no concept of it. We don't have the infrastructure to write those contracts yet. That's the gap SIL was built to address.

The need hasn't changed. What changed is our honest assessment of what we could actually do about it.

---

## Why We Stopped

### The safety funding thesis broke in real time.

Jaan Tallinn — our primary funder target, the man whose entire philosophy was to use capital as a counterweight to the race — funded Anthropic's Series A. In February 2026, [Anthropic revised its Responsible Scaling Policy](https://creati.ai/ai-news/2026-02-26/anthropic-responsible-scaling-policy-v3-safety-commitments-pentagon-2026/), loosening key safety commitments under competitive pressure from the rest of the field. The official framing: moving slower than competitors would make the world less safe. This is not a failure of Jaan's judgment — it is a description of the dynamics he was trying to counterbalance. The safety philanthropist ecosystem has been absorbed by the race it was trying to slow.

### The people building these systems are leaving, and saying why.

This is not a small thing. The researchers who designed the safety protocols at the major labs are resigning and publishing their reasons:

**[Jan Leike](https://venturebeat.com/ai/openais-former-superalignment-leader-blasts-company-safety-culture-and-processes-have-taken-a-backseat/)**, co-head of OpenAI's Superalignment team, resigned in May 2024: *"Safety culture and processes have taken a backseat to shiny products."* The entire Superalignment team — announced with a pledge to devote 20% of OpenAI's compute to it — was dissolved. By August 2024, nearly half of OpenAI's AGI safety team was gone.

**[Mrinank Sharma](https://www.eweek.com/news/ai-safety-leader-resigns-anthropic-global-risks/)**, Head of Safeguards Research at Anthropic, resigned February 2026. His letter: *"I have seen how hard it is to truly let our values govern our actions, both within myself and within institutions shaped by competition, speed, and scale."*

**[John Schulman](https://www.cnbc.com/2024/08/06/openai-co-founder-john-schulman-says-he-will-join-rival-anthropic.html)**, OpenAI co-founder, left in August 2024 to deepen his focus on AI alignment — moving to Anthropic, the lab most explicitly built around safety as a core constraint.

**[Miles Brundage](https://www.cnbc.com/2024/10/24/openai-miles-brundage-agi-readiness.html)**, OpenAI's Senior Advisor for AGI Readiness, resigned in October 2024, stating: *"Neither OpenAI nor any other frontier lab is ready, and the world is also not ready."* OpenAI disbanded the AGI Readiness team at the same time.

**[Geoffrey Hinton](https://www.aol.com/articles/godfather-ai-says-more-worried-155204412.html)**, the Godfather of AI, left Google in 2023 specifically to speak freely. As of late 2025, he says he is *"more worried"* than when he left.

These are not peripheral figures or outside critics. They are the people who built the safety protocols, designed the evaluations, and sat in the rooms where decisions were made. When they leave and say it isn't working, that is the most credible possible signal that it isn't working.

### The companies removed their own constraints, one by one.

OpenAI's original mission was to build AI that *"safely benefits humanity."* In its [2024 IRS disclosure](https://piunikaweb.com/2026/02/17/openai-mission-change-safely-removed-tax-filing/), the word "safely" was removed. [OpenAI shipped GPT-4.1 without a safety report](https://techcrunch.com/2025/04/15/openai-ships-gpt-4-1-without-a-safety-report/) — the documentation it had called *"a key part"* of accountability. It converted to a for-profit structure in 2025, with the nonprofit retaining roughly a quarter of stock in the new entity. [It then disbanded its Mission Alignment team in February 2026](https://techcrunch.com/2026/02/11/openai-disbands-mission-alignment-team-which-focused-on-safe-and-trustworthy-ai-development/), after 16 months of existence. Sam Altman at the [2024 NYT DealBook Summit](https://www.windowscentral.com/software-apps/sam-altman-agi-will-come-sooner-than-anticipated): *"My guess is that we will hit AGI sooner than most people think, and it will matter much less. A lot of the safety concerns actually don't come at the AGI moment."*

### The regulatory floor was removed on day one.

Trump revoked Biden's AI Safety Executive Order within hours of taking office in January 2025 — the order that had established mandatory safety testing and disclosure requirements for powerful models. The replacement explicitly reframes safety constraints as "ideological bias" and directs agencies to rescind anything that could impede development.

### The institution was ahead of the product.

SIF required a filed 501(c)(3), a founding board, 18 months of runway ($2-5M minimum), and a credible pitch to a billionaire who gets pitched constantly — all before a single conversation with Marion, Eric, or Jaan had happened. The one thing with demonstrated external traction was a code exploration tool with 8,800 downloads. Institutions follow demonstrated value. They don't precede it.

### The pace is simply incompatible with the institution we designed.

SIF was designed for a 50-year horizon. Four frontier AI models shipped from four companies in 25 days in late 2025. OpenAI raised $110 billion in a single round in February 2026. An institution built for generational durability requires a world stable enough for generational bets. That world is not this one.

---

## What Remains True

The direction was right. AI systems acting in the world without inspectable reasoning, without explicit authorization contracts, without any provenance trail for their decisions — that is a genuine infrastructure gap, and it is not getting smaller. The incidents above are early evidence. The systems are getting more capable faster than the contracts governing them are getting more careful.

The timescales and dollar scales of SIF's full vision were never going to happen on the schedule the problem demands. The race is moving too fast for patient capital. The philanthropist ecosystem that should fund this work has been absorbed into the labs it was supposed to counterbalance. The regulatory floor has been removed. The researchers with the deepest understanding of what's being built are the ones walking out.

A 50-year institution is the right answer to a 50-year problem. But we are not in a 50-year moment.

What we can do is smaller, more direct, and real: build the tools, publish the research, demonstrate the approach. [Semantic Infrastructure Lab](https://semanticinfrastructurelab.org) continues.

The clearest proof of concept is [Reveal](https://semanticinfrastructurelab.org/articles/reveal-introduction) — a semantic query layer for code, infrastructure, and documentation that lets AI systems inspect structure at any depth without reading files whole. It has 8,800 downloads and was built by one person, without a foundation, without philanthropic backing — because the problem was real enough to just build.

That is what SIL is: not an institution, but a demonstration. The glass-box collaboration model works and is documented. The work on behavioral contracts, provenance, and inspectable reasoning is available to anyone building systems where the blast radius of a wrong decision is real.

We gave up on the institution. Not the problem, and not the direction.

If any of this is relevant to what you're building: [scott@semanticinfrastructurelab.org](mailto:scott@semanticinfrastructurelab.org).

---

*— Scott Senkeresty*
*Founder, Semantic Infrastructure Foundation*
*March 2026*
