<script lang="ts">
  import { onMount } from 'svelte'

  // Illustrative severity per planning factor (0–1) → synthesized business-risk score.
  const factors = [
    { label: 'Inferred business purpose', value: 0.78 },
    { label: 'Service impact assessment', value: 0.88 },
    { label: 'Service dependencies & startup order', value: 0.60 },
    { label: 'Cross-server dependencies', value: 0.45 },
    { label: 'Known issues for the update', value: 0.70 },
  ]
  const SCORE = 72
  const LEVEL = 'High'

  function colorFor(v: number) {
    if (v >= 0.7) return '#f8444d'
    if (v >= 0.45) return '#f59e0b'
    return '#3b82f6'
  }

  let containerEl: HTMLDivElement
  let visibleRows = $state(0)
  let scoreVisible = $state(false)
  let score = $state(0)
  let timers: ReturnType<typeof setTimeout>[] = []

  const ROW_DELAY = 700

  onMount(() => {
    const section = containerEl.closest('section')

    function start() {
      stop()
      // reveal one bar at a time
      for (let i = 1; i <= factors.length; i++) {
        timers.push(setTimeout(() => { visibleRows = i }, i * ROW_DELAY))
      }
      // then reveal + count up the synthesized business risk
      timers.push(setTimeout(() => {
        scoreVisible = true
        const t0 = performance.now()
        const dur = 900
        function tick(now: number) {
          const p = Math.min(1, (now - t0) / dur)
          score = Math.round(p * SCORE)
          if (p < 1) requestAnimationFrame(tick)
        }
        requestAnimationFrame(tick)
      }, (factors.length + 1) * ROW_DELAY))
    }
    function stop() {
      timers.forEach(clearTimeout)
      timers = []
      visibleRows = 0
      scoreVisible = false
      score = 0
    }

    if (!section) { start(); return }

    const obs = new MutationObserver(() => {
      section.classList.contains('present') ? start() : stop()
    })
    obs.observe(section, { attributes: true, attributeFilter: ['class'] })
    if (section.classList.contains('present')) start()
    return () => { obs.disconnect(); stop() }
  })
</script>

<div class="ops" bind:this={containerEl}>
  <div class="bars">
    {#each factors as f, i}
      <div class="row" class:shown={i < visibleRows}>
        <span class="lbl">{f.label}</span>
        <div class="track">
          <div
            class="fill"
            style="width: {i < visibleRows ? f.value * 100 : 0}%; background: {colorFor(f.value)};"
          ></div>
        </div>
      </div>
    {/each}
  </div>

  <div class="arrow" style="opacity: {scoreVisible ? 1 : 0}; transition: opacity 0.4s;">→</div>

  <div class="scorecard" style="opacity: {scoreVisible ? 1 : 0}; transition: opacity 0.5s;">
    <p class="cap">Business Risk</p>
    <p class="score">{score}</p>
    <p class="level">{LEVEL}</p>
  </div>
</div>

<style>
  .ops {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 2rem;
    max-width: 900px;
    margin: 0 auto;
  }
  .bars {
    flex: 1;
    display: flex;
    flex-direction: column;
    gap: 1rem;
  }
  .row {
    display: flex;
    align-items: center;
    gap: 1rem;
    opacity: 0;
    transform: translateY(6px);
    transition: opacity 0.4s ease, transform 0.4s ease;
  }
  .row.shown {
    opacity: 1;
    transform: translateY(0);
  }
  .lbl {
    width: 17rem;
    flex-shrink: 0;
    text-align: left;
    font-size: 0.95rem;
    color: rgba(255, 255, 255, 0.8);
  }
  .track {
    flex: 1;
    height: 14px;
    background: rgba(255, 255, 255, 0.07);
    border-radius: 7px;
    overflow: hidden;
  }
  .fill {
    height: 100%;
    border-radius: 7px;
    width: 0;
    transition: width 0.9s cubic-bezier(0.22, 1, 0.36, 1);
  }
  .arrow {
    font-size: 2rem;
    color: rgba(255, 255, 255, 0.3);
    flex-shrink: 0;
  }
  .scorecard {
    flex-shrink: 0;
    width: 180px;
    text-align: center;
    background: rgba(248, 68, 77, 0.08);
    border: 1.5px solid rgba(248, 68, 77, 0.45);
    border-radius: 14px;
    padding: 1.25rem 1rem;
  }
  .cap {
    font-size: 0.7rem;
    font-weight: 600;
    text-transform: uppercase;
    letter-spacing: 0.08em;
    color: rgba(255, 255, 255, 0.55);
  }
  .score {
    font-size: 3.5rem;
    font-weight: 700;
    line-height: 1.1;
    color: #f8444d;
    font-family: "Mona Sans", sans-serif;
  }
  .level {
    font-size: 1.1rem;
    font-weight: 700;
    color: #f8444d;
  }
</style>
