<!-- Inline SVG icons for projects without logo images -->
<script>
  let { name, color } = $props();
</script>

<div class="icon-wrap" style="--accent: {color}">
  {#if name === 'duanleks'}
    <!-- Rocket / launchpad -->
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round">
      <path d="M12 2C12 2 7 6 7 12c0 2.5 1 4.5 2 6"/>
      <path d="M12 2c0 0 5 4 5 10c0 2.5-1 4.5-2 6"/>
      <circle cx="12" cy="10" r="2"/>
      <path d="M7 18c-1.5 0-3 1-3 3h16c0-2-1.5-3-3-3"/>
    </svg>
  {:else if name === 'cleaner'}
    <!-- Sparkle / clean -->
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round">
      <path d="M12 3L13.5 8.5L19 10L13.5 11.5L12 17L10.5 11.5L5 10L10.5 8.5Z"/>
      <path d="M19 3L19.8 5.2L22 6L19.8 6.8L19 9L18.2 6.8L16 6L18.2 5.2Z" stroke-width="1.4"/>
      <path d="M5 17L5.5 18.5L7 19L5.5 19.5L5 21L4.5 19.5L3 19L4.5 18.5Z" stroke-width="1.4"/>
    </svg>
  {:else if name === 'webpaint'}
    <!-- Paintbrush -->
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round">
      <path d="M18.37 2.63L14 7l-1.59-1.59a2 2 0 0 0-2.82 0L8 7l9 9 1.59-1.59a2 2 0 0 0 0-2.82L17 10l4.37-4.37a2.12 2.12 0 1 0-3-3z"/>
      <path d="M9 8c-2 3-4 3.5-7 4l8 8c1-.5 3.5-2.5 4-7"/>
      <path d="M14.5 17.5L4.5 15"/>
    </svg>
  {:else if name === 'searxng'}
    <!-- Magnifying glass with privacy shield -->
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round">
      <circle cx="10" cy="10" r="6"/>
      <line x1="14.5" y1="14.5" x2="21" y2="21"/>
      <path d="M10 7v3l2 1" stroke-width="1.4"/>
    </svg>
  {/if}
</div>

<style>
  .icon-wrap {
    width: 40px;
    height: 40px;
    border-radius: 8px;
    background: color-mix(in srgb, var(--accent) 12%, var(--bg-card, #13161d));
    border: 1px solid color-mix(in srgb, var(--accent) 22%, transparent);
    display: flex;
    align-items: center;
    justify-content: center;
    flex-shrink: 0;
  }

  svg {
    width: 20px;
    height: 20px;
    color: var(--accent);
  }

  @media (max-width: 640px) {
    .icon-wrap {
      width: 44px;
      height: 44px;
    }
  }
</style>
