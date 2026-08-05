<script lang="ts">
  // Placeholder for a real product screenshot Dom will drop in later.
  // Replace with an <img> once the capture exists.
  let { label = 'screenshot', hint = '' }: { label?: string; hint?: string } = $props()
</script>

<div class="shot">
  <div class="frame">
    <span class="cam">🖼️</span>
    <p class="label">{label}</p>
    {#if hint}<p class="hint">{hint}</p>{/if}
    <p class="tag">screenshot goes here</p>
  </div>
</div>

<style>
  .shot {
    max-width: 900px;
    margin: 0 auto;
  }
  .frame {
    height: 52vh;
    border: 2px dashed rgba(255, 255, 255, 0.2);
    border-radius: 12px;
    background: rgba(255, 255, 255, 0.02);
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 0.5rem;
  }
  .cam { font-size: 2.5rem; opacity: 0.5; }
  .label {
    font-size: 1.25rem;
    font-weight: 700;
    color: rgba(255, 255, 255, 0.75);
  }
  .hint {
    font-size: 0.95rem;
    color: rgba(255, 255, 255, 0.45);
    max-width: 32rem;
    text-align: center;
  }
  .tag {
    margin-top: 0.4rem;
    font-family: "IBM Plex Mono", monospace;
    font-size: 0.7rem;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    color: rgba(255, 255, 255, 0.3);
  }
</style>
