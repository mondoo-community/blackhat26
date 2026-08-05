<script lang="ts">
  // BlackHat 2026 — "This Is Fine: Securing AI While the Room's on Fire"
  //
  // Slides are being built fresh. The RSA 2026 deck lives at ../rsa26/src/Slides.svelte
  // (and its components in ../rsa26/src/components/) — reference it to reuse patterns,
  // components, and layouts. See ./README.md for the full Script.
  import MondooLogoFull from './components/MondooLogoFull.svelte'
  import VulnTrendChart from './components/VulnTrendChart.svelte'
  import ExploitWindowChart from './components/ExploitWindowChart.svelte'
  import thisIsFineImg from '/images/this-is-fine.webp'
  import vulns2026Img from '/images/vulns-2022-2026.png'
  import aiByDeptImg from '/images/ai-usage.by-department.webp'
  import aiByMaturityImg from '/images/ai-usage.by-maturity.webp'
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
      <h1 class="text-6xl font-bold">This Is Fine</h1>
      <h2 class="text-2xl mt-4 op-80 leading-snug">
        Securing AI<br />While the Room's on Fire
      </h2>
      <p class="mt-8 text-lg op-60">Dominik Richter</p>
      <p class="text-base op-40">dom@mondoo.com</p>
    </div>
  </div>
</section>

<!-- ============================================================= -->
<!-- Act 1 — The fastest-moving attack surface in history          -->
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
