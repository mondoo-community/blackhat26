# BlackHat 2026

Title: This Is Fine: Securing AI While the Room's on Fire
Tracks: AI, ML, & Data Science, Risk, Compliance and Security Management
Length: 20min
Format: Custom reveal.js-ish slides

## Abstract

Vulnerabilities are getting discovered faster than ever, with exploits releasing instantly. Attackers are rolling out automated attacks, and half of your organization is frantically wiring up every new AI tool and agent it can find. It's the fastest-moving attack surface in history.
Here's the security industry, sitting in the burning room with a cup of coffee: Scan, find, report, repeat. We've gotten extraordinary at producing findings and terrible at producing fixes. A dashboard full of risk is not a security program.

This talk is about getting out of the chair. We'll borrow a discipline engineers already trust and apply it to security with agentic AI. You'll see agents add real context to a risk, plan a reviewable fix, and ship it through the tools your team already uses, including securing the AI surface itself. The machine does the work. The human keeps the authority. In 20 minutes, we'll trade "this is fine" for the fire actually being out.

## Approach

- showcase 2-3 recent (up to 5 months ago) Vulnerabilities in AI Agents, AI plugins, or AI components
  - Research 5-7 interesting recent vulns affecting endpoints (Mac/Windows laptops particulary, developers)
  - Research 2-3 interesting recent vulns affecting server infrastructure
- showcase traditional vuln flows (eg: discover > aggregate > prioritize > ticket > report; or find more)
  - outline that it's way too little focus on getting things fixed
- showcase Mondoo's agentic vuln flow (analyze > plan > ship)
  - plan takes prioritized input but does the work traditionally left to security advisors
  - focus on remediations and mitigations to close everything that is found (always have an action to reduce risk)
  - operational considerations for things that are risky and would take time with operations
  - auto-create exceptions and justifications for things that can't move
  - risk assessment of the change itself
  - plan is the easy-mode of "what should I do next?"
  - plan in 3 flavors: thorough, trying to fix as much as you can, comprehensive; low-hanging / easy fixes only, everything low risk to move things; or normal
- think like a platform engineer
  - security at risk of being seen as a blocker, especially to business value when you have to move faster
  - agentic AI has vastly increased the participation in code from other departments
  - use agents to build automations or integrate into existing ones
  - automated flows are easier to audit, test, and revert


# Script

> Speaker: Dominik Richter · 20 minutes · Custom reveal.js-ish slides (reuse the RSA deck's
> components: `PipelineGraph`, `DiscoveryGraph`, `NutritionBadge`, `BlastRadiusGraph`,
> `AnalysisBlock`, `OpsConsiderations`, `AgentOverview`, `CumulativeChart`, plus new AI-specific
> blocks noted inline). Each entry below is roughly one slide: **Visual** = what's on screen,
> **Say** = the narration beat. Keep it fast — three fires, one escape.
>
> Through-line: *We got extraordinary at producing findings and terrible at producing fixes. The
> attack surface that's on fire right now is AI itself — on the endpoint, in the supply chain, in
> the pipeline. The way out is not another dashboard; it's agents that add context, plan a
> reviewable fix, and ship it through the tools you already run — the machine does the work, the
> human keeps the authority.*

---

## Act 0 — Cold open (≈1.5 min)

### 0.1 — Title / logo
- **Visual:** Mondoo logo → title card: **"This Is Fine: Securing AI While the Room's on Fire"**.
  Subtitle: *Agentic AI, turned on security.* Dominik Richter · dom@mondoo.com. Background: the
  "This is fine" dog-in-the-burning-room energy (starfield → embers).
- **Say:** "Quick show of hands — how many of you have shipped an AI coding agent to production, or
  onto your own laptop, in the last six months? Keep them up if your security team has an inventory
  of every one of those agents, every skill they've installed, and every credential they can reach.
  Right. That's the room. And it's on fire."

### 0.2 — The premise
- **Visual:** Three fast stats fade in.
  - Vulnerabilities discovered faster than ever, exploits release *instantly*.
  - Attackers run automated, agentic attacks.
  - Half your org is wiring up every AI tool and agent it can find.
- **Say:** "This is the fastest-moving attack surface in the history of computing. And here's the
  security industry, sitting in the burning room with a cup of coffee: **scan, find, report,
  repeat.** We've gotten *extraordinary* at producing findings and *terrible* at producing fixes.
  A dashboard full of risk is not a security program. This talk is about getting out of the chair."

---

## Act 1 — The room's on fire (≈3 min)

### 1.1 — The attack surface moved onto the endpoint
- **Visual:** A laptop icon lighting up with agent processes: shell, credentials, source, cloud CLIs,
  SSH keys, MCP servers. Pull-quote from Mondoo co-founder Dominik Richter.
- **Say:** "AI risk *used* to live in the cloud — model endpoints, APIs, data pipelines. Today it
  sits on the employee workstation. An AI agent on a dev laptop executes shell commands, reads your
  credentials, modifies source, and talks to your cloud. Developer laptops are now the most
  privileged, *least* monitored machines in the enterprise — GitHub tokens, cloud keys, SSH keys,
  package-publishing creds, all one prompt injection away."

### 1.2 — Shadow AI: you can't secure what you can't see
- **Visual:** "Shadow AI" wall. Fleet of ~25 agent/assistant logos (Claude Code, Cursor, Windsurf,
  Copilot, Gemini CLI, Cline, Kiro, Goose, Roo, OpenClaw, …). Overlay stats:
  - Only **29%** of orgs say they're ready to secure agentic AI.
  - **1,184** malicious skills found on a single skill registry (ClawHub / "ClawHavoc").
  - Of **58,730** AI agent skills scanned, only ~**20%** came back clean — **46,811** carried threats.
  - **492** MCP servers exposed to the internet with **zero authentication** (Trend Micro).
- **Say:** "Most of this adoption is unsanctioned and invisible. AI assistants are spreading across
  engineering faster than security can track them. This isn't hypothetical — one skill registry had
  over a thousand malicious skills and *nothing* flagged them. When Mondoo scanned ~59,000 skills,
  four out of five carried a threat. You cannot secure what you cannot see."

### 1.3 — What we'll cover
- **Visual:** Three fires, mirroring the RSA "We will cover" slide — one colored card each:
  - 🔴 **DuneSlide** — Cursor sandbox escape · CVE-2026-50548/50549 · CVSS 9.8 · the *endpoint*
  - 🔵 **mcp-remote RCE** — AI supply-chain infra · CVE-2025-6514 · CVSS 9.6 · the *server*
  - 🟣 **ClawHavoc** — malicious AI agent skills · shadow AI · the *inventory*
- **Say:** "Three real, recent fires. One on the endpoint, one in the server infrastructure, one in
  the AI supply chain itself. For each I'll show the same move: don't just *find* it — add context,
  plan the fix, and ship it. Then we turn that machinery on shadow AI itself."

### 1.4 — Before we start (framing)
- **Visual:** Three lines (reuse RSA "Before we start" layout).
  1. Basics of agentic AI — analyze → plan → ship
  2. **Context > Agent** — the model is cheap; the context is the moat
  3. Security best-practices — the human keeps the authority
- **Say:** "Two things to hold onto. First, the agent is not the interesting part — the *context* is.
  A generic model with your asset graph, your exploit intel, and your change history beats a genius
  model with none of it. Second: automated flows are easier to *audit, test, and revert* than a
  human doing it by hand at 2am. Keep those in mind."

### 1.5 — The agentic workflow (the spine)
- **Visual:** `PipelineGraph` — **Analyze → Plan → Ship**. This is the recurring backbone; each fire
  lights up one stage.
- **Say:** "Here's the spine of the whole talk. Analyze: add real context to a risk. Plan: do the
  work we usually hand to a senior security advisor. Ship: push the change through the tools your
  team already runs. Let's light it up."

---

## Act 2 — Fire #1: The endpoint (DuneSlide) (≈4 min)

### 2.1 — DuneSlide title
- **Visual:** `data-starfield`, red. **DuneSlide** · CVE-2026-50548 / CVE-2026-50549.
- **Say:** "Fire one is on the laptop. Cato AI Labs disclosed this on July 1st — a pair of flaws in
  Cursor, the AI code editor, they nicknamed DuneSlide."

### 2.2 — DuneSlide details (auto-advance fragments)
- **Visual:** Reuse the RSA per-vuln fragment layout.
  - **CVSS 9.8** — sandbox / command-execution bypass driven by prompt injection.
  - **Affected:** Cursor's agent sandbox — the layer meant to stop the AI from touching the OS.
  - **The trigger is content, not an exploit:** the agent reads a malicious repo / file / MCP
    response and the instructions *become a shell*. Related class: clone-a-repo → RCE with zero
    extra user steps (CVE-2026-26268); zero-click via MCP auto-start on Windsurf (CVE-2026-30615).
  - **Threats:** RCE on the developer's machine → tokens, cloud keys, SSH keys, source, supply chain.
- **Say:** "The scary part isn't a memory-corruption bug. There's no malicious attachment, no browser
  exploit. A *prompt* — hidden in a repo, a file, an MCP tool response — turns into code execution on
  the dev's box. When prompts become shells, the sandbox that was supposed to save you is the thing
  that gets bypassed."

### 2.3 — ANALYZE: light up the pipeline
- **Visual:** `PipelineGraph highlight={0}` → then `DiscoveryGraph`: discover *which* endpoints
  actually run Cursor, which versions, on macOS / Windows / Linux. (Mondoo added ~25 MQL resources +
  an AI Security policy with 38 checks to detect ~25 agents/assistants across the fleet.)
- **Say:** "Step one is not 'panic.' It's 'who actually has this?' We discover every endpoint running
  the vulnerable Cursor build across Mac, Windows, and Linux — the same way you'd inventory any other
  package, except now the package is an AI agent."

### 2.4 — ANALYZE: is this risk real? (nutrition badge)
- **Visual:** Walk the `NutritionBadge` factors exactly like the RSA deck, one at a time:
  - **Biz Impact** — whose laptop? A platform engineer with prod cloud keys vs. a kiosk.
  - **Atk Surface** — vulnerable version present *and* the agent is allowed to open untrusted repos.
  - **Exploits** — public PoC, prompt-injection is trivial to weaponize, zero/low interaction.
  - **Blast** — `BlastRadiusGraph`: from one dev laptop → git tokens, cloud creds, CI, package
    registries, internal services. A dev box is a skeleton key.
  - **News** — named vuln, broad coverage, CISA-adjacent attention on AI editors.
- **Say:** "Now we make it *real*. CVSS says 9.8 for everyone; context says this specific laptop
  belongs to someone who can deploy to prod. Look at the blast radius — one compromised dev machine
  is git tokens, cloud keys, and a straight line into CI and your package registry. *That's* the
  number that should drive the queue, not the CVSS."

### 2.5 — The fix looks easy… until it isn't
- **Visual:** Oppenheimer two-face (reuse): **Straightforward fix** ("just update Cursor") vs.
  **Operational reality** (auto-update disabled by policy, pinned versions, unmanaged BYOD laptops,
  the agent is mid-task on someone's release).
- **Say:** "'Just update it' — sure. Except a third of these are BYOD, some are version-pinned for a
  reason, and one is on the machine that's cutting tonight's release. This is exactly where security
  stalls out. So let's not stall."

### 2.6 — PLAN → SHIP: the endpoint fix
- **Visual:** `PipelineGraph highlight={2}` → a reviewable plan: force-update the managed fleet via
  **Intune / Kandji / CrowdStrike Falcon**, tighten Cursor's policy to deny untrusted-repo
  auto-run, and auto-file a scoped exception for the pinned/BYOD machines with a justification and a
  recheck date.
- **Say:** "The plan ships the update to every managed endpoint through Intune, Kandji, or
  CrowdStrike — tools you already run. For the machines that genuinely can't move tonight, the agent
  writes the exception *and* the justification *and* sets a recheck, instead of leaving a red row on
  a dashboard forever. Fire one: contained."

---

## Act 3 — Fire #2: The server / supply chain (mcp-remote) (≈3.5 min)

### 3.1 — mcp-remote title
- **Visual:** `data-starfield`, blue. **mcp-remote RCE** · CVE-2025-6514.
- **Say:** "Fire two moved off the laptop and into the plumbing. MCP — the Model Context Protocol —
  is how agents connect to tools and data. It's now core infrastructure, and it's got the security
  maturity of 1998."

### 3.2 — mcp-remote details
- **Visual:** Fragment layout.
  - **CVSS 9.6** — RCE in widely-deployed MCP infrastructure (`mcp-remote`).
  - Companion class: hooks/config injection in a popular coding agent (CVE-2025-59536), and hidden
    prompt injection in a PR description → RCE via GitHub Copilot (CVE-2025-53773, CVSS 9.6).
  - **Reach:** MCP sits *under* Cursor, VS Code, Windsurf, Claude Code, Gemini-CLI — "the mother of
    all AI supply chains." One flaw, everyone downstream.
  - **Threats:** server-side RCE, lateral movement, data exfiltration through the connectors agents
    trust by default.
- **Say:** "This is a supply-chain problem wearing an AI costume. The vulnerable component sits
  *beneath* every major agent, and 492 of these servers were sitting on the internet with no auth at
  all. When the shared substrate is vulnerable, everybody's fire."

### 3.3 — Why are fixes slow? (the honest slide)
- **Visual:** RSA-style question card: **"Why don't people take action on findings faster?"** Then
  `OpsConsiderations`.
- **Say:** "Here's the part vendors skip. Fixes are slow because they're *operationally expensive*.
  This MCP server brokers auth for three production agents. You can't just `kill -9` it at noon."

### 3.4 — PLAN: mitigation vs. fix (the advisor's work)
- **Visual:** Reuse the RSA layered-mitigation slide, re-skinned for MCP:
  1. **Network mitigation** (low disruption) — pull the server off the public internet, require auth
     at the gateway, block the exploit pattern.
  2. **Pin / disable risky connectors** (medium) — turn off auto-start and untrusted tool auto-run.
  3. **Upgrade the component** (high) — the real fix, needs a maintenance window per agent.
  4. **Rotate the brokered secrets** (high) — assume the tokens it held are burned.
- **Say:** "The agent doesn't just say 'patch it.' It does what a senior security advisor does: gives
  you a *ladder*. Cut exposure now for near-zero disruption, land the real upgrade in a window, and —
  because this thing brokered credentials — plan the rotation. Always an action that reduces risk
  *today*, even when the perfect fix has to wait."

### 3.5 — PLAN: pre-flight, rollback, and the risk of the change itself
- **Visual:** Reuse RSA Pre-flight / Rollback panels: pre-flight checks for IOCs / compromise before
  patching; rollback path documented; **plus a risk assessment of the change itself** (what breaks if
  the connector restarts mid-task).
- **Say:** "And the plan assesses *its own* blast radius — what happens to the three agents mid-flight
  when this restarts — and hands you a rollback. Automated flows are easier to audit, test, and
  revert than a human SSH-ing in under pressure. Fire two: laddered down and boxed in."

---

## Act 4 — Fire #3: Shadow AI itself (ClawHavoc) (≈4 min)

### 4.1 — ClawHavoc title
- **Visual:** `data-starfield`, purple. **ClawHavoc** — malicious AI agent skills at scale.
- **Say:** "Fire three is the one nobody has a map for. Your people are installing *skills* — little
  modular extensions — into their agents, from registries security has never heard of. This is
  shadow AI at the atomic level."

### 4.2 — ClawHavoc details
- **Visual:** Fragments.
  - **1,184** malicious skills discovered on one registry — **zero** detections fired.
  - A real "business automation" skill: silently read Gmail, sent mail with no per-action approval,
    posted to social, routed traffic through untrusted hosts → **14 findings, 8 High/Critical.**
  - Attack classes: prompt injection / hidden instructions, credential theft, data exfiltration,
    SSRF, approval-fatigue abuse — mapped to **OWASP LLM Top 10** & **MITRE ATLAS**.
  - **Widely deployed, massive blast radius, and completely invisible to the fleet.**
- **Say:** "One 'helpful' skill quietly reads your inbox, sends mail without asking, and phones home
  through hosts you've never seen — and it does it *inside* an agent your developer already trusts.
  A thousand of these were live with nothing catching them. This is the surface the old scanner
  never even looked at."

### 4.3 — You can't secure what you can't inventory → AIBOM
- **Visual:** New block (extend `AgentOverview`): generate an **AI Bill of Materials**. `cnspec aibom`
  inventories models, agents, skills, MCP servers, guardrails, knowledge bases → exports **CycloneDX
  ML-BOM**. Show it as the SBOM-for-AI equivalent of the RSA "Code-security 101" beat.
- **Say:** "Engineers already solved 'what's in my software' — it's called an SBOM. So we do the same
  for AI. One command produces an AI-BOM: every model, agent, skill, MCP server, guardrail, and
  knowledge base, in a standard format. Now shadow AI has a shape you can query."

### 4.4 — Scan the skills before they land (Skill Check)
- **Visual:** Mondoo **AI Skill Check** — free, agent-agnostic, 6-layer pipeline (static + YARA +
  shell taint → ML prompt-injection classifier → LLM behavior-mismatch → deep inspection → FP filter
  → verdict with OWASP-LLM / MITRE ATLAS mapping). "Scan before you install."
- **Say:** "Then we check every skill the way you'd check a dependency — six layers, from regex and
  taint-tracking up to an LLM that catches a skill whose *documentation* lies about what it does.
  Verdict comes back mapped to OWASP and ATLAS. This is Code-security 101, pointed at AI."

### 4.5 — SHIP: preventive control, not another alert
- **Visual:** `PipelineGraph highlight={2}` → Mondoo's July 2026 **shadow AI** capability, four beats:
  1. **Endpoint AI inventory (AI-BOM)** — every agent, skill, plugin, MCP server, model on the fleet.
  2. **Continuous risk assessment** — each component vs. known vulns *and* your governance policy.
  3. **AI policy control** — define which AI tools are *allowed* on company endpoints.
  4. **Operational remediation** — remove unauthorized agents / bad skills via **Intune** &
     **CrowdStrike Falcon**.
- **Say:** "Most tools watch agentic activity at runtime and *tell you after it happened*. That's one
  more alert in the burning room. Mondoo's move is **preventive control** — govern which AI tooling
  and which capabilities are even allowed on the endpoint, and rip out the unsanctioned ones through
  Intune and CrowdStrike *before* they do damage. Fire three doesn't get monitored. It gets put out."

---

## Act 5 — The human keeps the authority (≈3 min)

### 5.1 — Auto-pilot, not un-manned
- **Visual:** `AgentOverview` — the three flavors of Plan (reuse RSA framing):
  **Comprehensive** (fix as much as possible), **Low-hanging** (only low-risk, everything that moves
  freely), **Normal** (balanced). Every plan is reviewable, versioned, revertible.
- **Say:** "The Plan is easy-mode for 'what do I do next?' Pick your appetite: fix everything, or
  just the low-risk wins, or balanced. Whatever it proposes is a *reviewable diff* — the machine does
  the work, you keep the authority. Nobody's handing the org over to an autonomous agent; you're
  handing it the toil."

### 5.2 — Think like a platform engineer
- **Visual:** Three lines.
  - Security is at risk of being seen as the *blocker* — especially when the business has to move fast.
  - Agentic AI massively widened who writes code / touches infra — non-engineers are shipping now.
  - So meet them there: agents build the automations, or plug into the ones you already have.
- **Say:** "Borrow the discipline engineers already trust. Everything we did tonight shipped through
  Ansible, Terraform, Intune, Kandji, CrowdStrike — no new console, no new silo. Automated flows are
  auditable, testable, and revertible. That's how security stops being the blocker and starts being
  the thing that lets the business move fast *safely*."

### 5.3 — Stay in control / the fire's actually out
- **Visual:** Control-room background (reuse) → `CumulativeChart`: findings *fixed* over time, not
  findings *found*. The "This is fine" dog finally puts the coffee down and walks out of a room that's
  no longer on fire.
- **Say:** "A dashboard full of risk was never the goal — *fixed* risk is. Attackers move in hours,
  not weeks; this gives defenders the same speed, with the context, transparency, and control to
  trust it. In twenty minutes we traded 'this is fine' for the fire actually being out."

### 5.4 — Close
- **Visual:** Mondoo logo. **Booth 2411** · mondoo.com · LinkedIn /in/dominikrichter. Tagline:
  *"Fix now. Fix forever."*
- **Say:** "Come find me at booth 2411 — I'll show you the AI-BOM of your own laptop, live, and
  you can decide how much of the room is on fire. Thank you."

---

## Reference facts & sources (for slide accuracy)

**Mondoo shadow-AI / agentic timeline**
- **Apr 2026** — first shadow-AI detection: AI Security policy (**38 checks**), ~**25** new MQL
  resources detecting ~**25** agents/assistants (Claude, Codex, Copilot, Gemini CLI, Windsurf, Zed,
  Cline, Continue, Kiro, Goose, Junie, Augment, Warp, Kilo Code, OpenHands, Qwen Code, Antigravity,
  IBM Bob, OpenClaw, Snowflake Cortex, Roo, Trae, OpenCode, Pi, Mistral Vibe).
  — https://mondoo.com/docs/releases/2026-04-mondoo-release-highlights
- **May 2026** — scans Claude Code, Claude Desktop, Cursor, Windsurf, Kiro IDE, Roo Code for known
  vulns (macOS/Windows/Debian/Ubuntu); native providers for Anthropic, OpenAI, Mistral, Hugging Face,
  Ollama, Together AI, vLLM; `cnspec aibom` → CycloneDX ML-BOM.
  — https://mondoo.com/docs/releases/2026-05-mondoo-release-highlights
- **Jun 2026** — cloud AI-workload coverage: AWS (Amazon Q Business, Bedrock), GCP (Vertex AI Agent
  Engine, RAG corpora), OCI (Generative AI + AI Agents), DigitalOcean GradientAI.
  — https://mondoo.com/docs/releases/2026-06-mondoo-release-highlights
- **Jul 29 2026** — "Mondoo Expands Vulnerability Management to Shadow AI": endpoint AI-BOM,
  continuous risk assessment, AI policy control, operational remediation via Intune & CrowdStrike
  Falcon; **preventive control** vs. runtime monitoring; Richter quote ("AI risk used to be centered
  in the cloud … today it sits on the employee workstation"); 300+ customers, OWASP GenAI silver
  sponsor. — https://www.globenewswire.com/news-release/2026/07/29/3335253/0/en/mondoo-expands-vulnerability-management-to-shadow-ai.html
- **AI Skill Check** — free, agent-agnostic, **6-layer** pipeline (59 regex + 13 YARA rules, shell
  taint, archive/Unicode/binary analysis → ML prompt-injection → LLM behavior-mismatch → deep
  inspection → FP filter → verdict w/ OWASP-LLM Top 10 + MITRE ATLAS). **28** threat types / 6
  classes. Public stats: **58,730** skills scanned, ~**20%** clean (11,919), **46,811** with threats.
  ClawHavoc: **1,184** malicious skills on ClawHub; one automation skill → 14 findings, 8 High/Crit.
  Only **29%** of orgs ready to secure agentic AI; **492** MCP servers exposed w/ zero auth (Trend Micro).
  — https://mondoo.com/ai-agent-security · https://mondoo.com/blog/introducing-mondoo-ai-skills-check · https://mondoo.com/skillcheck
- **Agentic VM vision** — prioritization / orchestration / remediation; Ansible + Terraform + Intune;
  versioned, reviewable, instant rollback; "Fix now. Fix forever."; "Attackers move in hours, not
  weeks." — https://mondoo.com/newsroom/mondoo-raises-17-5-million-to-pioneer-agentic-vulnerability-management
- **MCP for Mondoo** — https://mondoo.com/blog/introducing-mcp-for-mondoo-unlocking-ai-to-fix-cves-faster ·
  Anthropic Cyber Verification Program — https://mondoo.com/blog/mondoo-accepted-anthropic-cyber-verification-program

**Featured vulnerabilities (verify CVE IDs/scores before the talk)**
- **DuneSlide** (Cursor) — CVE-2026-50548 / CVE-2026-50549, CVSS 9.8, sandbox/command-exec bypass via
  prompt injection, disclosed Jul 1 2026 by Cato AI Labs. Related: CVE-2026-26268 (clone-repo → RCE,
  zero extra steps); Windsurf CVE-2026-30615 (zero-click via MCP auto-start).
  — https://www.catonetworks.com/blog/curxecute-rce/ · https://cybersecuritynews.com/cursor-ai-coding-agent-vulnerability/ · https://www.csoonline.com/article/4191923/
- **mcp-remote RCE** — CVE-2025-6514, CVSS 9.6, RCE in widely-deployed MCP infra. Related:
  CVE-2025-59536 (hooks/config injection); GitHub Copilot CVE-2025-53773, CVSS 9.6 (prompt injection
  in PR desc → RCE); Microsoft Semantic Kernel prompt-injection → host RCE.
  — https://www.microsoft.com/en-us/security/blog/2026/05/07/prompts-become-shells-rce-vulnerabilities-ai-agent-frameworks/ · https://www.ox.security/blog/the-mother-of-all-ai-supply-chains-critical-systemic-vulnerability-at-the-core-of-the-mcp/
- **ClawHavoc** — malicious AI agent skills at scale (see AI Skill Check sources above).

