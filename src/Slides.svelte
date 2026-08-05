<script lang="ts">
  // BlackHat 2026 — "This Is Fine: Securing AI While the Room's on Fire"
  //
  // Slides are being built fresh. The RSA 2026 deck lives at ../rsa26/src/Slides.svelte
  // (and its components in ../rsa26/src/components/) — reference it to reuse patterns,
  // components, and layouts. See ./README.md for the full Script.
  import MondooLogoFull from './components/MondooLogoFull.svelte'
  import VulnTrendChart from './components/VulnTrendChart.svelte'
  import ExploitWindowChart from './components/ExploitWindowChart.svelte'
  import AibomBlock from './components/AibomBlock.svelte'
  import SkillCheckPipeline from './components/SkillCheckPipeline.svelte'
  import OpsRiskMeter from './components/OpsRiskMeter.svelte'
  import NutritionBadge from './components/NutritionBadge.svelte'
  import BlastRadiusGraph from './components/BlastRadiusGraph.svelte'
  import PipelineGraph from './components/PipelineGraph.svelte'
  import AgentOverview from './components/AgentOverview.svelte'
  import CumulativeChart from './components/CumulativeChart.svelte'
  import ShotPlaceholder from './components/ShotPlaceholder.svelte'
  import thisIsFineImg from '/images/this-is-fine.webp'
  import vulns2026Img from '/images/vulns-2022-2026.png'
  import aiByDeptImg from '/images/ai-usage.by-department.webp'
  import aiByMaturityImg from '/images/ai-usage.by-maturity.webp'
  import shadowAiGraphImg from '/images/mondoo.shadow-ai-graph.png'
  import aiSkillRisksImg from '/images/ai.skill.risks.png'
  import workflowIntuneImg from '/images/workflow.intune.png'
  import workflowEasmImg from '/images/workflow.easm.png'
  import planGoalsImg from '/images/plan.goals.png'

  const aiFactors = [
    { name: 'business',   label: 'Biz Impact',  impact: 1, isActive: true, factor: 'business' as const },
    { name: 'surface',    label: 'Atk Surface', impact: 1, isActive: true, factor: 'surface' as const },
    { name: 'exploit',    label: 'Exploits',    impact: 1, isActive: true, factor: 'exploit' as const },
    { name: 'connection', label: 'Blast',       impact: 1, isActive: true, factor: 'connection' as const },
    { name: 'news',       label: 'News',        impact: 1, isActive: true, factor: 'news' as const },
  ]
</script>

<section data-starfield>
  <MondooLogoFull width={555} height={108} />
</section>

<section data-starfield>
  <div class="flex items-center justify-center gap-12 max-w-5xl mx-auto">
    <img
      src={thisIsFineImg}
      alt="This is fine"
      style="width: 420px; max-width: 40vw; height: auto; border-radius: 8px; display: block; flex-shrink: 0;"
    />
    <div class="text-left">
      <h1 class="text-5xl font-bold leading-tight">
        Securing AI<br />While the<br />Room is<br />on Fire
      </h1>
      <p class="mt-8 text-lg op-60">Dominik Richter</p>
      <p class="text-base op-40">dom@mondoo.com</p>
    </div>
  </div>
</section>

<!-- ============================================================= -->
<!-- Cold open — the room's on fire                                -->
<!-- ============================================================= -->

<section data-starfield>
  <h1 class="text-5xl font-bold">The fastest-moving</h1>
  <h1 class="text-5xl font-bold mt-2">attack surface in history</h1>
</section>

<!-- 1: Vulnerability discovery rates -->
<section>
  <h2 class="text-3xl font-bold mb-2">Vulnerabilities keep rocketing higher</h2>
  <p class="text-lg op-60 mb-4">48,177 CVEs in 2025 — a record, broken again · ≈ one every 11 minutes</p>
  <VulnTrendChart />
</section>

<!-- 1b: ...and then 2026 — the explosion in critical/high vulns -->
<section>
  <h2 class="text-3xl font-bold mb-2">…and then 2026 happened</h2>
  <p class="text-lg op-60 mb-4">Critical &amp; high-severity CVEs from 21 major vendors — the curve went vertical</p>
  <img
    src={vulns2026Img}
    alt="Critical and high severity vulnerabilities from 21 notable organizations, 2022–2026"
    style="max-width: 74%; max-height: 62vh; height: auto; border-radius: 8px; background: #fff; padding: 0.75rem; display: block; margin: 0 auto;"
  />
  <p class="text-sm op-40 mt-3">Source: Epoch AI (CC-BY)</p>
</section>

<!-- 2: Exploit speed — the collapse from years to hours -->
<section>
  <h2 class="text-3xl font-bold mb-1">And the window to exploit is collapsing</h2>
  <p class="text-lg op-60 mb-4">Time from disclosure to working exploit — years to hours</p>

  <ExploitWindowChart />

  <p class="text-lg op-70 mt-6 max-w-3xl mx-auto">
    AI generates a working exploit from a CVE advisory in <span class="font-bold" style="color: #f8444d;">~10–15 min</span> for <span class="font-bold" style="color: #f8444d;">~$1</span>
  </p>
  <p class="text-xs op-30 mt-3">
    Mean TTE 2018–2025; 2026 YTD median —
    <a href="https://cyberunit.com/insights/cve-exploit-window-10-hours-ai-attackers-2026/" target="_blank" rel="noopener noreferrer" style="color: rgba(255,255,255,0.4); text-decoration: underline;">cyberunit.com</a>
  </p>
</section>

<!-- 3: Explosion in AI usage (the pivot) — two fragments: everywhere, then ungoverned -->
<section>
  <h2 class="text-3xl font-bold mb-6">And your whole org is wiring up AI</h2>
  <div style="position: relative; height: 60vh; max-width: 1000px; margin: 0 auto;">
    <figure class="fragment fade-out" data-fragment-index="0" style="position: absolute; inset: 0; margin: 0; display: flex; flex-direction: column; align-items: center; justify-content: center;">
      <img
        src={aiByDeptImg}
        alt="Share of respondents using AI by business function"
        style="max-width: 100%; max-height: 50vh; height: auto; border-radius: 8px; background: #fff; padding: 0.75rem; display: block;"
      />
      <figcaption class="text-lg op-70 mt-3">Every business function — not just engineering</figcaption>
    </figure>
    <figure class="fragment" data-fragment-index="0" style="position: absolute; inset: 0; margin: 0; display: flex; flex-direction: column; align-items: center; justify-content: center;">
      <img
        src={aiByMaturityImg}
        alt="Phase of AI use by business function"
        style="max-width: 100%; max-height: 50vh; height: auto; border-radius: 8px; background: #fff; padding: 0.75rem; display: block;"
      />
      <figcaption class="text-lg op-70 mt-3">…but mostly experimenting &amp; piloting — almost none fully scaled or governed</figcaption>
    </figure>
  </div>
</section>

<!-- ============================================================= -->
<!-- Act 1 — Shadow AI: map the environment, uncover the vulnerable -->
<!-- ============================================================= -->

<section data-starfield>
  <p class="text-2xl op-50 mb-3">Act 1</p>
  <h1 class="text-6xl font-bold">Shadow AI</h1>
  <p class="text-2xl mt-6 op-70">You can't secure what you can't see</p>
</section>

<!-- 1.0 — The problem: unsanctioned, invisible, on the endpoint -->
<section data-autoslide="1000">
  <h2 class="text-3xl font-bold mb-3">AI risk moved onto the endpoint</h2>
  <p class="text-lg op-60 mb-10 max-w-3xl mx-auto">Unsanctioned, invisible, and holding your credentials</p>
  <div class="flex justify-center gap-6 flex-wrap max-w-5xl mx-auto">
    <div class="fragment stat" data-fragment-index="0">
      <p class="text-5xl font-bold" style="color: #ec4899;">73%</p>
      <p class="text-base op-60 mt-2">of CISOs fear<br/>AI agent risks</p>
    </div>
    <div class="fragment stat" data-fragment-index="1">
      <p class="text-5xl font-bold" style="color: #f8444d;">30%</p>
      <p class="text-base op-60 mt-2">have mature<br/>safeguards in place</p>
    </div>
    <div class="fragment stat" data-fragment-index="2">
      <p class="text-5xl font-bold" style="color: #a855f7;">4 / 5</p>
      <p class="text-base op-60 mt-2">of ~59K skills scanned<br/>carried a threat</p>
    </div>
    <div class="fragment stat" data-fragment-index="3" data-autoslide="9999999">
      <p class="text-5xl font-bold" style="color: #60a5fa;">492</p>
      <p class="text-base op-60 mt-2">MCP servers exposed<br/>with zero auth</p>
    </div>
  </div>
  <p class="text-xs op-30 mt-8">
    73% / 30%: NeuralTrust, State of AI Agent Security 2026 · skill stats: Mondoo AI Skill Check · 492: Trend Micro
  </p>
</section>

<!-- 1.1 — Build the map: the AI-BOM -->
<section>
  <h2 class="text-3xl font-bold mb-2">Build the map: an AI Bill of Materials</h2>
  <p class="text-lg op-60 mb-6">The SBOM, pointed at AI — one command, every agent, skill, MCP server &amp; model</p>
  <AibomBlock />
</section>

<!-- 1.1b — See the map: the Shadow AI graph (product) -->
<section>
  <h2 class="text-3xl font-bold mb-2">Inventory your AI footprint</h2>
  <p class="text-lg op-60 mb-4">Understand the relationships between AI agents, skills, MCP servers and the data they can access</p>
  <img
    src={shadowAiGraphImg}
    alt="Mondoo Shadow AI graph — relationships between assets, agents, skills and repositories"
    style="max-width: 88%; max-height: 66vh; height: auto; border-radius: 8px; border: 1px solid rgba(255,255,255,0.1); display: block; margin: 0 auto;"
  />
</section>

<!-- 1.2 — Layer 1: endpoint agents → DuneSlide (Cursor) -->
<section data-starfield>
  <p class="text-xl op-50 mb-2">Layer 1 · Endpoint agents</p>
  <h1 class="text-5xl font-bold" style="color: #f8444d;">DuneSlide</h1>
  <p class="text-2xl mt-4 op-70">CVE-2026-50548 / 50549</p>
</section>

<section data-starfield data-autoslide="1200">
  <h2 class="text-3xl font-bold mb-8" style="color: #f8444d;">DuneSlide</h2>
  <div class="text-left max-w-3xl mx-auto text-lg">
    <p class="fragment" data-fragment-index="0" data-autoslide="1200"><span class="font-bold">CVSS 9.8</span> <span class="op-70">— sandbox / command-execution bypass in Cursor, driven by prompt injection</span></p>
    <p class="fragment mt-4" data-fragment-index="1" data-autoslide="1200"><span class="font-bold">The trigger is content, not an exploit</span> <span class="op-70">— the agent reads a malicious repo / file / MCP response and the instructions <em>become a shell</em></span></p>
    <p class="fragment mt-4" data-fragment-index="2" data-autoslide="1200"><span class="font-bold">Same class:</span> <span class="op-70">clone-a-repo → RCE (CVE-2026-26268); zero-click via MCP auto-start on Windsurf (CVE-2026-30615)</span></p>
    <p class="fragment mt-4" data-fragment-index="3" data-autoslide="9999999"><span class="font-bold">Threats:</span> <span class="op-70">RCE on the dev's machine → tokens, cloud keys, SSH keys, source, supply chain</span></p>
  </div>
</section>

<!-- 1.3 — Context, not just a CVSS -->
<section>
  <h2 class="text-3xl font-bold mb-6" style="color: #f8444d;">Context &gt; CVSS</h2>
  <div class="mb-8">
    <NutritionBadge highlight={['connection']} shown={['business', 'surface', 'exploit', 'news']} categories={aiFactors} />
  </div>
  <p class="text-lg op-70 max-w-3xl mx-auto">CVSS 9.8 for everyone — context says <span class="font-bold">this</span> laptop deploys to prod</p>
</section>

<section>
  <h2 class="text-3xl font-bold mb-3" style="color: #f8444d;">Blast radius — one dev laptop is a skeleton key</h2>
  <BlastRadiusGraph data={{
    asset: { title: 'Dev laptop', subtitle: 'Cursor / DuneSlide', risk: 100 },
    local: [
      { title: 'Credentials', type: 'secrets', count: 3, children: [
        { title: 'GitHub token', risk: 90 },
        { title: 'Cloud keys', risk: 90 },
        { title: 'SSH keys', risk: 80 },
      ]},
      { title: 'Source', type: 'database', count: 2, children: [
        { title: 'Monorepo', risk: 80 },
        { title: '.env files', risk: 90 },
      ]},
    ],
    remote: [
      { title: 'CI / CD', type: 'webserver', count: 2, children: [
        { title: 'pipelines', risk: 80 },
        { title: 'runners', risk: 70 },
      ]},
      { title: 'Registries', type: 'webserver', count: 2, children: [
        { title: 'npm publish', risk: 90 },
        { title: 'container reg', risk: 80 },
      ]},
    ],
  }} />
</section>

<!-- 1.4 — Layer 2: the AI supply chain → mcp-remote -->
<section data-starfield>
  <p class="text-xl op-50 mb-2">Layer 2 · The AI supply chain</p>
  <h1 class="text-5xl font-bold" style="color: #3b82f6;">mcp-remote</h1>
  <p class="text-2xl mt-4 op-70">CVE-2025-6514</p>
</section>

<section data-starfield data-autoslide="1200">
  <h2 class="text-3xl font-bold mb-8" style="color: #3b82f6;">mcp-remote</h2>
  <div class="text-left max-w-3xl mx-auto text-lg">
    <p class="fragment" data-fragment-index="0" data-autoslide="1200"><span class="font-bold">CVSS 9.6</span> <span class="op-70">— RCE in widely-deployed MCP infrastructure</span></p>
    <p class="fragment mt-4" data-fragment-index="1" data-autoslide="1200"><span class="font-bold">Sits <em>under</em> every agent</span> <span class="op-70">— Cursor, VS Code, Windsurf, Claude Code, Gemini-CLI. One flaw, everyone downstream</span></p>
    <p class="fragment mt-4" data-fragment-index="2" data-autoslide="1200"><span class="font-bold">492 servers</span> <span class="op-70">exposed to the internet with zero auth — "the mother of all AI supply chains"</span></p>
    <p class="fragment mt-4" data-fragment-index="3" data-autoslide="9999999"><span class="font-bold">Threats:</span> <span class="op-70">server-side RCE, lateral movement, data exfiltration via trusted connectors</span></p>
  </div>
</section>

<!-- 1.5 — Layer 3: skills → ClawHavoc + Skill Check -->
<section data-starfield>
  <p class="text-xl op-50 mb-2">Layer 3 · Skills &amp; plugins</p>
  <h1 class="text-5xl font-bold" style="color: #a855f7;">ClawHavoc</h1>
  <p class="text-2xl mt-4 op-70">Malicious AI agent skills, at scale</p>
</section>

<section>
  <h2 class="text-3xl font-bold mb-4" style="color: #a855f7;">AI Skills Security</h2>
  <img
    src={aiSkillRisksImg}
    alt="Mondoo AI Agent Security — skills risk intelligence"
    style="max-width: 84%; max-height: 60vh; height: auto; border-radius: 8px; border: 1px solid rgba(255,255,255,0.1); display: block; margin: 0 auto;"
  />
  <p class="text-sm op-40 mt-3">
    <a href="https://mondoo.com/ai-agent-security/skills" target="_blank" rel="noopener noreferrer" style="color: rgba(255,255,255,0.5); text-decoration: underline;">mondoo.com/ai-agent-security/skills</a>
  </p>
</section>

<section>
  <h2 class="text-3xl font-bold mb-2" style="color: #a855f7;">Scan every skill like a dependency</h2>
  <p class="text-base op-60 mb-6">A "business automation" skill: read Gmail, sent mail with no approval, phoned home through untrusted hosts</p>
  <SkillCheckPipeline />
</section>

<!-- 1.6 — The map is complete -->
<section data-autoslide="1000">
  <h2 class="text-4xl font-bold mb-8">Now you see the fire 🔥</h2>
  <div class="flex justify-center gap-8 flex-wrap max-w-4xl mx-auto text-left">
    <div class="fragment fire" data-fragment-index="0" style="border-color: rgba(248,68,77,0.4);">
      <p class="text-xl font-bold" style="color: #f8444d;">🔴 DuneSlide</p>
      <p class="op-60 mt-1">endpoint agents · Cursor</p>
    </div>
    <div class="fragment fire" data-fragment-index="1" style="border-color: rgba(59,130,246,0.4);">
      <p class="text-xl font-bold" style="color: #3b82f6;">🔵 mcp-remote</p>
      <p class="op-60 mt-1">AI supply chain · MCP</p>
    </div>
    <div class="fragment fire" data-fragment-index="2" style="border-color: rgba(168,85,247,0.4);">
      <p class="text-xl font-bold" style="color: #a855f7;">🟣 ClawHavoc</p>
      <p class="op-60 mt-1">skills &amp; plugins</p>
    </div>
  </div>
  <h2 class="fragment text-4xl font-bold mt-12" data-fragment-index="3" data-autoslide="9999999">Time to get it fixed.</h2>
</section>

<!-- ============================================================= -->
<!-- Act 2 — Automated remediation: put the three fires out         -->
<!-- ============================================================= -->

<section data-starfield>
  <p class="text-2xl op-50 mb-3">Act 2</p>
  <h1 class="text-6xl font-bold">Automated remediation</h1>
  <p class="text-2xl mt-6 op-70">Put the fires out</p>
</section>

<!-- 2.0 — From findings to fixes: the Plan -->
<section>
  <h2 class="text-3xl font-bold mb-2">A finding isn't a fix</h2>
  <p class="text-lg op-60 mb-8">The plan is the connecting glue between prioritized findings and shipping fixes</p>
  <PipelineGraph highlight={1} />
</section>

<!-- 2.0c — The anatomy of fixing (points to the 3 upcoming sections) -->
<section>
  <h2 class="text-4xl font-bold mb-10">The anatomy of fixing</h2>
  <div class="flex justify-center gap-6 flex-wrap max-w-5xl mx-auto text-left">
    <div class="anatomy" style="border-color: rgba(59,130,246,0.4);">
      <div class="flex items-baseline gap-3">
        <span class="num" style="color: #3b82f6;">1</span>
        <p class="text-2xl font-bold">Goals</p>
      </div>
      <p class="op-60 anatomy-desc">What are we optimizing for?</p>
    </div>
    <div class="anatomy" style="border-color: rgba(168,85,247,0.4);">
      <div class="flex items-baseline gap-3">
        <span class="num" style="color: #a855f7;">2</span>
        <p class="text-2xl font-bold">Mitigation vs Remediation</p>
      </div>
      <p class="op-60 anatomy-desc">Reduce risk now, or fix for good.</p>
    </div>
    <div class="anatomy" style="border-color: rgba(236,72,153,0.4);">
      <div class="flex items-baseline gap-3">
        <span class="num" style="color: #ec4899;">3</span>
        <p class="text-2xl font-bold">Operational Risks</p>
      </div>
      <p class="op-60 anatomy-desc">Pre-flight, rollback, blast radius.</p>
    </div>
  </div>
</section>

<!-- 2.0b — Goals -->
<section>
  <h2 class="text-3xl font-bold mb-4">1. Set your goals</h2>
  <img
    src={planGoalsImg}
    alt="Choose a goal — Reduce Risk, Quick Wins, or MTTR Reduction"
    style="max-width: 84%; max-height: 62vh; height: auto; border-radius: 8px; border: 1px solid rgba(255,255,255,0.1); display: block; margin: 0 auto;"
  />
</section>

<!-- 2.2 — Mitigation vs Remediation (the ladder) -->
<section>
  <h2 class="text-3xl font-bold mb-8">2. Mitigation vs Remediation</h2>
  <div class="text-left max-w-3xl mx-auto text-base">
    <p class="fragment op-80" data-fragment-index="0"><span class="font-bold" style="color: #06b6d4;">1. Network mitigation</span> <span class="op-50">(low disruption)</span> — off the public internet, auth at the gateway, block the pattern</p>
    <p class="fragment mt-3 op-80" data-fragment-index="1"><span class="font-bold" style="color: #3b82f6;">2. Disable risky connectors</span> <span class="op-50">(medium)</span> — no auto-start, no untrusted-tool auto-run</p>
    <p class="fragment mt-3 op-80" data-fragment-index="2"><span class="font-bold" style="color: #a855f7;">3. Upgrade the component</span> <span class="op-50">(high)</span> — the real fix, needs a maintenance window per agent</p>
    <p class="fragment mt-3 op-80" data-fragment-index="3"><span class="font-bold" style="color: #ec4899;">4. Rotate brokered secrets</span> <span class="op-50">(high)</span> — assume the tokens it held are burned</p>
  </div>
  <p class="fragment text-lg op-70 mt-8" data-fragment-index="3">Always an action that reduces risk <em>today</em> — even when the perfect fix has to wait</p>
</section>

<!-- 2.3 — Operational Risks: planning inputs → business risk -->
<section>
  <h2 class="text-3xl font-bold mb-3">3. Operational Risks</h2>
  <p class="text-lg op-60 mb-10">As part of planning we analyze and consider — synthesized into the business risk of the update</p>
  <OpsRiskMeter />
</section>

<!-- Automated Exceptions -->
<section data-autoslide="1200">
  <h2 class="text-3xl font-bold mb-3">Automated Exceptions</h2>
  <p class="text-lg op-60 mb-10 max-w-3xl mx-auto">When the operational risk means a fix can't move yet, the plan requests the exception — instead of leaving a dead red row</p>
  <div class="flex justify-center gap-5 flex-wrap max-w-5xl mx-auto text-left">
    <div class="fragment excard" data-fragment-index="0">
      <p class="font-bold text-xl">Rule-driven</p>
      <p class="op-60 mt-2">Tedious &amp; manual today → partly automated with good rules</p>
    </div>
    <div class="fragment excard" data-fragment-index="1">
      <p class="font-bold text-xl">Time-boxed</p>
      <p class="op-60 mt-2">Limited-time only — expiry suggested automatically</p>
    </div>
    <div class="fragment excard" data-fragment-index="2" data-autoslide="9999999">
      <p class="font-bold text-xl">Every type</p>
      <p class="op-60 mt-2">False positives and risk acceptance alike</p>
    </div>
  </div>
</section>

<!-- Pre-flight + Rollback -->
<section>
  <h2 class="text-3xl font-bold mb-8" style="color: #3b82f6;">Pre-flight + Rollback</h2>
  <div class="flex gap-8 max-w-4xl mx-auto text-left text-base">
    <div class="fragment flex-1" data-fragment-index="0" style="background: rgba(255,255,255,0.04); border: 1.5px solid rgba(168,85,247,0.4); border-radius: 12px; padding: 1.5rem;">
      <h3 class="font-bold text-xl mb-4 text-center" style="color: #a855f7;">Pre-flight</h3>
      <p class="op-80">Check for active compromise (IOCs) first</p>
      <p class="op-80 mt-3">Confirm scope &amp; target health</p>
      <p class="op-80 mt-3">Change approval + maintenance window</p>
      <p class="op-80 mt-3">Backups verified before anything changes</p>
      <p class="op-60 mt-4 text-sm">=&gt; Already breached? Abort → Incident Response</p>
    </div>
    <div class="fragment flex-1" data-fragment-index="1" style="background: rgba(255,255,255,0.04); border: 1.5px solid rgba(59,130,246,0.4); border-radius: 12px; padding: 1.5rem;">
      <h3 class="font-bold text-xl mb-4 text-center" style="color: #3b82f6;">Rollback</h3>
      <p class="op-80">Is rollback even possible? <span class="op-50 text-sm">(reversible vs. one-way)</span></p>
      <p class="op-80 mt-3">What must be backed up first? <span class="op-50 text-sm">(config, keys, data, state)</span></p>
      <p class="op-80 mt-3">How does the restore work? <span class="op-50 text-sm">(tested, step-by-step)</span></p>
      <p class="op-80 mt-3">Expected downtime? <span class="op-50 text-sm">(time to restore)</span></p>
    </div>
  </div>
</section>

<!-- 2.5 — Ship: automated workflows -->
<section>
  <h2 class="text-3xl font-bold mb-2">Automated workflows</h2>
  <p class="text-lg op-60 mb-8">Set up automated workflows that tackle the majority of your findings</p>
  <PipelineGraph highlight={2} />
</section>

<!-- The workflows -->
<section>
  <h2 class="text-3xl font-bold mb-4">The workflow</h2>
  <img
    src={workflowIntuneImg}
    alt="Mondoo Intune Remediation workflow"
    style="max-width: 82%; max-height: 60vh; height: auto; border-radius: 8px; border: 1px solid rgba(255,255,255,0.1); display: block; margin: 0 auto;"
  />
  <p class="text-lg op-70 mt-4">Autonomous remediation and rollout</p>
</section>

<section>
  <h2 class="text-3xl font-bold mb-4">The workflow</h2>
  <img
    src={workflowEasmImg}
    alt="Mondoo EASM Remediation workflow"
    style="max-width: 82%; max-height: 60vh; height: auto; border-radius: 8px; border: 1px solid rgba(255,255,255,0.1); display: block; margin: 0 auto;"
  />
  <p class="text-lg op-70 mt-4">Branch, verify, and stop safely — across the external attack surface</p>
</section>

<!-- Preventive control -->
<section>
  <h2 class="text-3xl font-bold mb-3" style="color: #a855f7;">🟣 Preventive control — not another alert</h2>
  <p class="text-lg op-60 mb-10">Set up rules for autonomous workflows</p>
  <div class="flex justify-center items-center gap-4 max-w-5xl mx-auto">
    <div class="fragment flex flex-col gap-3" data-fragment-index="0">
      <div class="fbox" style="border-color: rgba(59,130,246,0.5);">Analyze inventory</div>
      <div class="fbox" style="border-color: rgba(168,85,247,0.5);">Fixes</div>
      <div class="fbox" style="border-color: rgba(236,72,153,0.5);">Operational Risks</div>
    </div>
    <div class="fragment arrow" data-fragment-index="1">→</div>
    <div class="fragment decision" data-fragment-index="1">
      <p class="deci-cap">Decision</p>
      <p class="op-70 text-sm mt-1">Goals + change-risk appetite</p>
    </div>
    <div class="fragment arrow" data-fragment-index="2">→</div>
    <div class="fragment runbox" data-fragment-index="2">
      <p class="font-bold text-lg">▶ Run the workflow</p>
    </div>
  </div>
</section>

<!-- Automation you can trust -->
<section>
  <h2 class="text-3xl font-bold mb-2">Automation you can trust</h2>
  <p class="text-lg op-60 mb-4">Ships through the tools your team already runs — versioned, reviewable, revertible</p>
  <AgentOverview />
</section>

<!-- 2.6 — Stay in control -->
<section>
  <h2 class="text-3xl font-bold mb-2">Fixed &gt; found</h2>
  <p class="text-base op-50 mb-6">The machine does the work — you keep the authority</p>
  <CumulativeChart />
</section>

<!-- 2.7 — Close -->
<section data-starfield>
  <MondooLogoFull width={555} height={108} />
  <p class="mt-8 text-2xl op-70">Booth 5100 · AI Zone</p>
  <p class="mt-4 text-2xl op-70">
    <a href="https://mondoo.com" target="_blank" rel="noopener noreferrer" style="color: var(--link-color); text-decoration: none;">mondoo.com</a>
  </p>
  <p class="mt-8 text-xl op-80" style="display: inline-flex; align-items: center; gap: 0.4rem;">
    <a href="https://www.linkedin.com/in/dominikrichter" target="_blank" rel="noopener noreferrer" style="display: inline-flex; align-items: center; gap: 0.4rem; color: var(--link-color); text-decoration: none;">
      <svg width="22" height="22" viewBox="0 0 24 24" fill="currentColor">
        <path d="M20.45 20.45h-3.56v-5.57c0-1.33-.02-3.04-1.85-3.04-1.85 0-2.14 1.45-2.14 2.94v5.67H9.34V9h3.42v1.56h.05c.48-.9 1.64-1.85 3.38-1.85 3.61 0 4.28 2.38 4.28 5.47v6.27zM5.34 7.43a2.07 2.07 0 1 1 0-4.14 2.07 2.07 0 0 1 0 4.14zM7.12 20.45H3.56V9h3.56v11.45zM22.22 0H1.77C.79 0 0 .77 0 1.72v20.56C0 23.23.79 24 1.77 24h20.45c.98 0 1.78-.77 1.78-1.72V1.72C24 .77 23.2 0 22.22 0z"/>
      </svg>
      <span>dominikrichter</span>
    </a>
  </p>
</section>

<style>
  .stat {
    background: rgba(255, 255, 255, 0.03);
    border: 1px solid rgba(255, 255, 255, 0.08);
    border-radius: 12px;
    padding: 1.25rem 1.5rem;
    text-align: center;
    min-width: 180px;
  }
  .fire {
    background: rgba(255, 255, 255, 0.03);
    border: 1.5px solid rgba(255, 255, 255, 0.1);
    border-radius: 12px;
    padding: 1.25rem 1.75rem;
    min-width: 220px;
  }
  .flavor {
    background: rgba(255, 255, 255, 0.03);
    border: 1.5px solid rgba(255, 255, 255, 0.1);
    border-radius: 12px;
    padding: 1rem 1.5rem;
    min-width: 210px;
    text-align: center;
  }
  .anatomy {
    position: relative;
    background: rgba(255, 255, 255, 0.03);
    border: 1.5px solid rgba(255, 255, 255, 0.1);
    border-radius: 12px;
    padding: 1.5rem 1.75rem;
    width: 250px;
  }
  .anatomy .num {
    font-family: "IBM Plex Mono", monospace;
    font-size: 1.75rem;
    font-weight: 700;
  }
  .anatomy-desc {
    position: absolute;
    top: calc(100% + 0.9rem);
    left: 0;
    width: 100%;
    padding: 0 0.25rem;
  }
  .panel {
    flex: 1;
    background: rgba(255, 255, 255, 0.04);
    border: 1.5px solid rgba(255, 255, 255, 0.1);
    border-radius: 12px;
    padding: 1.25rem;
    text-align: center;
  }
  .step {
    background: rgba(255, 255, 255, 0.04);
    border: 1.5px solid rgba(255, 255, 255, 0.1);
    border-radius: 10px;
    padding: 0.9rem 1.1rem;
    min-width: 150px;
  }
  .arrow {
    display: flex;
    align-items: center;
    font-size: 1.5rem;
    color: rgba(255, 255, 255, 0.3);
  }
  .fbox {
    background: rgba(255, 255, 255, 0.04);
    border: 1.5px solid rgba(255, 255, 255, 0.1);
    border-radius: 10px;
    padding: 0.9rem 1.4rem;
    font-weight: 600;
    min-width: 210px;
    text-align: center;
  }
  .decision {
    background: rgba(168, 85, 247, 0.1);
    border: 1.5px solid rgba(168, 85, 247, 0.5);
    border-radius: 10px;
    padding: 1.1rem 1.4rem;
    min-width: 220px;
    text-align: center;
  }
  .decision .deci-cap {
    font-family: "IBM Plex Mono", monospace;
    font-size: 0.7rem;
    font-weight: 700;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    color: #c99bf5;
  }
  .runbox {
    background: rgba(112, 199, 72, 0.1);
    border: 1.5px solid rgba(112, 199, 72, 0.5);
    border-radius: 10px;
    padding: 1.1rem 1.4rem;
    min-width: 200px;
    text-align: center;
    color: #70c748;
  }
  .excard {
    background: rgba(255, 255, 255, 0.03);
    border: 1.5px solid rgba(255, 255, 255, 0.1);
    border-radius: 12px;
    padding: 1.5rem 1.75rem;
    width: 270px;
  }
</style>
