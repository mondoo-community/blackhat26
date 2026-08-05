<script lang="ts">
  // `cnspec aibom` — inventories the AI supply chain and exports a CycloneDX ML-BOM.
  // Counts are illustrative sample-fleet numbers for the demo slide.
  const rows = [
    { label: 'Agents',          count: 37,  note: 'Cursor · Claude Code · Windsurf · Copilot · …' },
    { label: 'Skills',          count: 612, note: 'ClawHub · Skills.sh · GitHub · Claude Marketplace' },
    { label: 'MCP servers',     count: 41,  note: '12 internet-facing' },
    { label: 'Models',          count: 28,  note: 'hosted + local' },
    { label: 'Guardrails',      count: 9,   note: '' },
    { label: 'Knowledge bases', count: 15,  note: '' },
  ]
</script>

<div class="term">
  <div class="bar">
    <span class="dot" style="background:#ff5f56"></span>
    <span class="dot" style="background:#ffbd2e"></span>
    <span class="dot" style="background:#27c93f"></span>
  </div>
  <div class="body">
    <p class="cmd"><span class="prompt">$</span> cnspec aibom <span class="flag">--output cyclonedx</span></p>
    <p class="muted">Inventorying AI supply chain across fleet…</p>
    <table>
      {#each rows as r}
        <tr>
          <td class="check">✓</td>
          <td class="lbl">{r.label}</td>
          <td class="cnt">{r.count}</td>
          <td class="note">{r.note}</td>
        </tr>
      {/each}
    </table>
    <p class="out">→ <span class="file">ai-bom.cdx.json</span> <span class="badge">CycloneDX ML-BOM</span><span class="cursor">▋</span></p>
  </div>
</div>

<style>
  .term {
    max-width: 760px;
    margin: 0 auto;
    background: #0c0c16;
    border: 1px solid rgba(255,255,255,0.1);
    border-radius: 10px;
    overflow: hidden;
    text-align: left;
    box-shadow: 0 20px 60px rgba(0,0,0,0.5);
  }
  .bar {
    display: flex;
    gap: 0.4rem;
    padding: 0.6rem 0.8rem;
    background: rgba(255,255,255,0.04);
    border-bottom: 1px solid rgba(255,255,255,0.08);
  }
  .dot { width: 11px; height: 11px; border-radius: 50%; }
  .body {
    padding: 1.1rem 1.3rem;
    font-family: "IBM Plex Mono", monospace;
    font-size: 1rem;
    line-height: 1.7;
  }
  .cmd { color: #fff; }
  .prompt { color: #a855f7; margin-right: 0.4rem; }
  .flag { color: rgba(255,255,255,0.45); }
  .muted { color: rgba(255,255,255,0.4); margin-bottom: 0.5rem; }
  table { width: 100%; border-collapse: collapse; }
  td { padding: 0.05rem 0; vertical-align: baseline; }
  .check { color: #70c748; width: 1.4rem; }
  .lbl { color: rgba(255,255,255,0.85); width: 9rem; }
  .cnt { color: #ec4899; font-weight: 700; text-align: right; width: 3.5rem; padding-right: 1rem; }
  .note { color: rgba(255,255,255,0.35); font-size: 0.82rem; }
  .out { margin-top: 0.7rem; color: rgba(255,255,255,0.7); }
  .file { color: #60a5fa; }
  .badge {
    margin-left: 0.6rem;
    font-size: 0.72rem;
    padding: 0.1rem 0.5rem;
    border: 1px solid rgba(168,85,247,0.5);
    border-radius: 999px;
    color: #c99bf5;
  }
  .cursor {
    color: #a855f7;
    margin-left: 0.3rem;
    animation: blink 1.1s steps(1) infinite;
  }
  @keyframes blink { 50% { opacity: 0; } }
</style>
