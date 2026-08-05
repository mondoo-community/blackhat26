<script lang="ts">
  // Mondoo AI Skill Check — 6-layer analysis pipeline.
  const layers = [
    { n: 1, title: 'Static analysis',       desc: '59 regex + 13 YARA · shell taint · archive/Unicode/binary' },
    { n: 2, title: 'ML classifier',         desc: 'prompt-injection detection' },
    { n: 3, title: 'LLM threat analysis',   desc: 'behavior-mismatch — does the code match the docs?' },
    { n: 4, title: 'Deep inspection',       desc: 'complex / ambiguous threats' },
    { n: 5, title: 'False-positive filter', desc: 'cross-layer review' },
    { n: 6, title: 'Verdict',               desc: 'mapped to OWASP LLM Top 10 + MITRE ATLAS' },
  ]
</script>

<div class="pipe">
  <div class="io in">skill.md&nbsp; +&nbsp; scripts / MCP config&nbsp; →</div>
  {#each layers as l (l.n)}
    <div class="layer" class:verdict={l.n === 6} style={`width: ${100 - (l.n - 1) * 7}%`}>
      <span class="num">L{l.n}</span>
      <span class="title">{l.title}</span>
      <span class="desc">{l.desc}</span>
    </div>
  {/each}
  <div class="io out">→ <span class="danger">DENIED</span> · credential-theft + data-exfil · 8 High/Critical</div>
</div>

<style>
  .pipe {
    max-width: 720px;
    margin: 0 auto;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 0.5rem;
  }
  .io {
    font-family: "IBM Plex Mono", monospace;
    font-size: 0.9rem;
    color: rgba(255,255,255,0.55);
  }
  .io.out { margin-top: 0.4rem; }
  .danger { color: #f8444d; font-weight: 700; }
  .layer {
    display: flex;
    align-items: baseline;
    gap: 0.9rem;
    padding: 0.55rem 1.1rem;
    background: rgba(255,255,255,0.04);
    border: 1px solid rgba(255,255,255,0.1);
    border-radius: 8px;
    text-align: left;
  }
  .layer.verdict {
    background: rgba(248,68,77,0.08);
    border-color: rgba(248,68,77,0.45);
  }
  .num {
    font-family: "IBM Plex Mono", monospace;
    font-size: 0.8rem;
    color: #a855f7;
    font-weight: 700;
    flex-shrink: 0;
  }
  .layer.verdict .num { color: #f8444d; }
  .title {
    font-weight: 700;
    color: #fff;
    font-size: 1rem;
    flex-shrink: 0;
  }
  .desc {
    color: rgba(255,255,255,0.5);
    font-size: 0.82rem;
  }
</style>
