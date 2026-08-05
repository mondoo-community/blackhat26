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
  import NutritionBadge from './components/NutritionBadge.svelte'
  import BlastRadiusGraph from './components/BlastRadiusGraph.svelte'
  import thisIsFineImg from '/images/this-is-fine.webp'
  import vulns2026Img from '/images/vulns-2022-2026.png'
  import aiByDeptImg from '/images/ai-usage.by-department.webp'
  import aiByMaturityImg from '/images/ai-usage.by-maturity.webp'
  import shadowAiGraphImg from '/images/mondoo.shadow-ai-graph.png'

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
</style>
