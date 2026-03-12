<script>
  let { status } = $props();

  const map = {
    active:   { label: "live",     dot: "#4ade80" },
    building: { label: "building", dot: "#fbbf24" },
    paused:   { label: "paused",   dot: "#6b7280" },
    archived: { label: "archived", dot: "#ef4444" },
  };

  const badge = $derived(map[status] ?? map.paused);
</script>

<span class="status-badge" style="--dot: {badge.dot}">
  <span class="dot" class:pulse={status === 'active'}></span>
  {badge.label}
</span>

<style>
  .status-badge {
    display: inline-flex;
    align-items: center;
    gap: 5px;
    font-family: 'JetBrains Mono', monospace;
    font-size: 10px;
    font-weight: 500;
    letter-spacing: 0.08em;
    text-transform: uppercase;
    color: var(--dot);
    opacity: 0.85;
  }

  .dot {
    width: 6px;
    height: 6px;
    border-radius: 50%;
    background: var(--dot);
    flex-shrink: 0;
  }

  .dot.pulse {
    box-shadow: 0 0 0 0 var(--dot);
    animation: dot-pulse 2.5s ease-out infinite;
  }

  @keyframes dot-pulse {
    0% { box-shadow: 0 0 0 0 color-mix(in srgb, var(--dot) 70%, transparent); }
    70% { box-shadow: 0 0 0 5px transparent; }
    100% { box-shadow: 0 0 0 0 transparent; }
  }
</style>
