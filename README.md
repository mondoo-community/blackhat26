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

> Speaker: Dominik Richter · 20 minutes · Custom reveal.js-ish slides. Reuse RSA deck components
> (`PipelineGraph`, `DiscoveryGraph`, `NutritionBadge`, `BlastRadiusGraph`, `AnalysisBlock`,
> `OpsConsiderations`, `AgentOverview`, `CumulativeChart`) plus the new Act-1 charts already built
> (`VulnTrendChart`, `ExploitWindowChart`, the AI-usage images). Each entry ≈ one slide:
> **Visual** = what's on screen, **Say** = the narration beat.
>
> **Two acts, one arc.** Act 1 — *Shadow AI*: you can't secure what you can't see, so we map the
> whole AI surface — endpoints, supply chain, skills — and uncover the vulnerable agents and skills
> hiding in it. Act 2 — *Automated remediation*: take those exact findings and put the fires out —
> a reviewable plan, an automated workflow, operational considerations, and rollback. The machine
> does the work; the human keeps the authority.
>
> **The three recurring fires** (same colors throughout both acts):
> - 🔴 **DuneSlide** — Cursor sandbox escape · CVE-2026-50548/50549 · CVSS 9.8 · the *endpoint agent*
> - 🔵 **mcp-remote RCE** — CVE-2025-6514 · CVSS 9.6 · the *AI supply chain (MCP)*
> - 🟣 **ClawHavoc** — malicious AI agent skills at scale · the *skills/plugins*

---

## Cold open — the room's on fire (≈2.5 min)
*(Already built in `Slides.svelte`: title → fastest-moving attack surface → CVE trend → 2026 explosion → exploit-window collapse → AI-everywhere.)*

### C.1 — Title
- **Visual:** "This is fine" image left, title right — **This Is Fine: Securing AI While the Room's on Fire.**
- **Say:** "Show of hands — who's shipped an AI coding agent onto their laptop in the last six months?
  Keep it up if security has an inventory of every one, every skill it installed, and every
  credential it can reach. Right. That's the room, and it's on fire."

### C.2 — The fastest-moving attack surface
- **Visual:** `VulnTrendChart` (48,177 CVEs in 2025) → the 2026 explosion image → `ExploitWindowChart`
  (~2.3 yrs → ~10 hrs) → the AI-usage-by-department / by-maturity swap.
- **Say:** "Vulnerabilities discovered faster than ever. The window to exploit collapsed from years
  to hours — because AI writes the exploit for a dollar. And every department is wiring up AI, almost
  none of it governed. Meanwhile the security industry sits with a coffee: scan, find, report, repeat.
  We got *extraordinary* at producing findings and *terrible* at producing fixes. Let's fix that — in
  two moves. First: **see it.** Then: **put it out.**"

---

## Act 1 — Shadow AI: map the environment, uncover the vulnerable (≈8 min)
*Focus: visibility and mapping. We build the map of the AI surface and surface the vulnerable agents & skills hiding in it. No remediation yet — that's Act 2.*

### 1.0 — You can't secure what you can't see
- **Visual:** The shadow-AI stat graphic in `static/images`. Overlay: only **29%** of orgs ready to
  secure agentic AI; **1,184** malicious skills on one registry with **zero** detections; of **58,730**
  skills scanned only ~**20%** clean; **492** MCP servers on the internet with no auth.
- **Say:** "AI risk used to live in the cloud. Today it sits on the employee workstation — an agent
  that runs shell commands, reads your credentials, and talks to your cloud. Most of it is
  unsanctioned and invisible. You cannot secure what you cannot see. So Act 1 is one job: **see it.**"

### 1.1 — Build the map: the AI-BOM
- **Visual:** `cnspec aibom` output → an inventory of every model, agent, skill, MCP server, guardrail,
  and knowledge base across the fleet, exported as a standard **CycloneDX ML-BOM**. Frame it as the
  SBOM-for-AI. (Mondoo: ~25 MQL resources + an AI Security policy of 38 checks; detects ~25
  agents/assistants across macOS/Windows/Linux.)
- **Say:** "Engineers already solved 'what's in my software' — the SBOM. We do the same for AI. One
  command, and shadow AI has a *shape you can query*: every agent, every skill, every MCP server,
  every model. That map is the whole game. Now let's walk its three layers — and find a fire on each."

### 1.1b — See the map: the Shadow AI graph
- **Visual:** `src/static/images/mondoo.shadow-ai-graph.png` — Mondoo's **Shadow AI → Graph**: agents,
  assets, skills, MCP servers, and repos as one relationship map, colored by **risk** (critical→none)
  and bordered by **status** (managed / allowed / discovered / denied). Node inspector on a selected
  agent (Windsurf / Codeium): **High** risk, status **Denied**, 16 assets, 42 skills, 5 MCP servers,
  1 vulnerability.
- **Say:** "This is what that map actually looks like. Every agent, every skill, every MCP server on
  the fleet — and the *lines between them*. Color is risk; the ring is whether it's sanctioned,
  discovered, or already denied. Click one agent and you see its whole footprint: how many machines it
  touches, how many skills it pulled in, which MCP servers it can reach. This is shadow AI, finally
  visible — and it's how we find each of the next three fires."

### 1.2 — Layer 1: endpoint agents → 🔴 DuneSlide (Cursor)
- **Visual:** `DiscoveryGraph` — which endpoints run Cursor, which versions, on which OS. Then the
  DuneSlide facts (fragments): **CVSS 9.8**, sandbox/command-exec bypass via prompt injection
  (CVE-2026-50548/50549, Cato AI Labs, Jul 1 2026). Related: clone-a-repo → RCE (CVE-2026-26268),
  zero-click via MCP auto-start on Windsurf (CVE-2026-30615).
- **Say:** "Layer one: the agents themselves. The map shows exactly which laptops run the vulnerable
  Cursor build. The bug? No memory corruption, no attachment — a *prompt* hidden in a repo becomes a
  shell on the dev's machine. When prompts become shells, the sandbox is the thing that gets bypassed."

### 1.3 — Context, not just a CVSS (make it real)
- **Visual:** `NutritionBadge` walked one factor at a time, ending on `BlastRadiusGraph` from a single
  dev laptop → git tokens, cloud creds, CI, package registries, internal services.
- **Say:** "CVSS says 9.8 for everyone. Context says *this* laptop belongs to someone who deploys to
  prod. One dev box is a skeleton key — git tokens, cloud keys, a straight line into CI and your
  registry. That blast radius is what should drive the queue. Visibility isn't a list; it's a *map
  with risk on it.*"

### 1.4 — Layer 2: the AI supply chain → 🔵 mcp-remote
- **Visual:** The map zooms out to MCP servers and connectors beneath every agent. Facts: **CVSS 9.6**
  RCE in widely-deployed `mcp-remote` (CVE-2025-6514); **492** exposed with zero auth. "The mother of
  all AI supply chains." Related: hooks/config injection (CVE-2025-59536); Copilot PR-description
  prompt injection → RCE (CVE-2025-53773).
- **Say:** "Layer two sits *underneath* everything — MCP, how agents reach tools and data. One flaw in
  the shared substrate and everyone downstream is exposed. The map doesn't just show you *have* MCP
  servers; it shows the vulnerable ones, and the ones you accidentally left facing the internet."

### 1.5 — Layer 3: skills & plugins → 🟣 ClawHavoc
- **Visual:** Mondoo **AI Skill Check** — 6-layer pipeline (static + YARA + shell taint → ML
  prompt-injection → LLM behavior-mismatch → deep inspection → FP filter → verdict, mapped to
  **OWASP LLM Top 10** + **MITRE ATLAS**). Case: a "business automation" skill that read Gmail, sent
  mail with no approval, phoned home through untrusted hosts → **14 findings, 8 High/Critical.**
- **Say:** "Layer three is the atomic one — *skills*. Little extensions your people install from
  registries you've never heard of, running inside an agent they already trust. We scan each one like
  a dependency — six layers, right up to an LLM that catches a skill whose *documentation lies* about
  what it does. A thousand malicious ones were live with nothing catching them. Now they're on the map."

### 1.6 — The map is complete
- **Visual:** Full AI-BOM, risk-scored: agents, MCP servers, skills, models — the three fires flagged
  🔴🔵🟣 in context.
- **Say:** "That's Act 1. Every AI agent, skill, and server across the fleet — inventoried, risk-scored,
  and the vulnerable ones surfaced. We can finally *see* the room. But a map of the fire is still not
  the fire being out. So — Act 2."

---

## Act 2 — Automated remediation: put the three fires out (≈8 min)
*Focus: take the exact findings from Act 1 and remediate them — a reviewable plan, an automated workflow, operational considerations, and rollback. `[SCREENSHOT]` = where Dom drops real plan/workflow captures.*

### 2.0 — From findings to fixes: the Plan
- **Visual:** `PipelineGraph` **Analyze → Plan → Ship**, highlight **Plan**. Plan is "easy-mode for
  *what do I do next?*" in three flavors: **Comprehensive** (fix as much as possible), **Low-hanging**
  (only low-risk, keep things moving), **Normal** (balanced). **`[SCREENSHOT — the Plan view]`**
- **Say:** "A finding isn't a fix. The Plan does the work we usually hand a senior security advisor:
  it turns each item on the map into an action. Pick your appetite — fix everything, just the easy
  wins, or balanced. And whatever it proposes is a *reviewable diff*, not a black box."

### 2.1 — Fix 🔴 DuneSlide (endpoint agent)
- **Visual:** The plan for Cursor: force-update the managed fleet via **Intune / Kandji / CrowdStrike
  Falcon**; tighten **AI policy control** to deny untrusted-repo auto-run; auto-file a scoped
  **exception + justification + recheck date** for pinned/BYOD machines. **`[SCREENSHOT — workflow: endpoint update]`**
- **Say:** "The obvious fix — 'just update Cursor' — dies on contact with reality: BYOD laptops,
  version pins, one machine mid-release. So the plan ships the update through the tools you already
  run, tightens the agent's policy so a repo can't auto-run, and for the machines that genuinely can't
  move tonight it writes the exception *and* the justification *and* a recheck — instead of a red row
  that lives forever."

### 2.2 — Fix 🔵 mcp-remote (supply chain): mitigation ≠ fix
- **Visual:** `OpsConsiderations` + the mitigation ladder:
  1. **Network mitigation** (low disruption) — off the public internet, auth at the gateway, block the pattern.
  2. **Disable risky connectors** (medium) — no auto-start, no untrusted-tool auto-run.
  3. **Upgrade the component** (high) — the real fix, needs a maintenance window per agent.
  4. **Rotate the brokered secrets** (high) — assume the tokens it held are burned.
- **Say:** "Fixes are slow because they're *operationally expensive* — this server brokers auth for
  three production agents; you can't `kill -9` it at noon. So the plan gives a *ladder*: cut exposure
  now for near-zero disruption, land the real upgrade in a window, and rotate the credentials it
  touched. There's always an action that reduces risk *today*, even when the perfect fix has to wait."

### 2.3 — Fix 🔵 mcp-remote: pre-flight, rollback, risk-of-the-change
- **Visual:** Pre-flight / Rollback panels — pre-flight IOC/compromise checks before patching; documented
  rollback; **a risk assessment of the change itself** (what breaks if the connector restarts mid-task).
  **`[SCREENSHOT — workflow: staged rollout + rollback]`**
- **Say:** "And the plan assesses its *own* blast radius — what happens to those three agents when this
  restarts — and hands you a rollback before it touches anything. Automated flows are easier to audit,
  test, and revert than a human SSH-ing in under pressure at 2am."

### 2.4 — Fix 🟣 ClawHavoc (skills): preventive control
- **Visual:** Mondoo's shadow-AI remediation, four beats: endpoint **AI-BOM** → **continuous risk
  assessment** vs. vulns + governance policy → **AI policy control** (which tools are *allowed*) →
  **operational remediation** — remove unauthorized agents / bad skills via **Intune** & **CrowdStrike
  Falcon**. Contrast: **preventive control**, not runtime monitoring.
- **Say:** "Most tools watch skills at runtime and tell you *after* it happened — one more alert in the
  burning room. We do the opposite: govern which AI tooling and which capabilities are even *allowed*
  on the endpoint, and rip the unsanctioned ones out through the tools you already run — *before* they
  do damage. The skill fire doesn't get monitored. It gets put out."

### 2.5 — Automation you can trust
- **Visual:** The remediation shipping through **Ansible · Terraform · Intune · Kandji · CrowdStrike** —
  no new console, no new silo. Everything versioned, reviewable, revertible. **`[SCREENSHOT — workflow overview]`**
- **Say:** "Notice what we did *not* do: buy another dashboard. Every fix went out through the platform
  your team already trusts. That's the whole trick — agentic AI has pulled half the org into shipping
  code and infra; meet them there, and automated flows become auditable, testable, and revertible."

### 2.6 — Stay in control (the human keeps authority)
- **Visual:** `CumulativeChart` — findings *fixed* over time, not findings *found*. The "This is fine"
  dog puts the coffee down and walks out of a room that's no longer on fire.
- **Say:** "A dashboard full of risk was never the goal — *fixed* risk is. The machine does the work;
  you keep the authority — every action a reviewable diff, every change revertible. Attackers move in
  hours; this gives defenders the same speed, with the context and control to trust it."

### 2.7 — Close
- **Visual:** Mondoo logo · **Booth 5100 · AI Zone** · mondoo.com · LinkedIn /in/dominikrichter · *"Fix now. Fix forever."*
- **Say:** "Two moves: see the whole AI surface, then put the fires out automatically. Come by booth
  5100 in the AI Zone — I'll build the AI-BOM of your own laptop, live, and you can decide how much of *your* room is
  on fire. Thank you."

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
