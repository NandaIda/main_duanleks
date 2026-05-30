<script>
  import { projects } from '$lib/projects.js';
  import ProjectCard from '$lib/ProjectCard.svelte';
  import { theme } from '$lib/theme.js';

  let selectedFilter = $state('all');
  const filters = ["all", "active", "building", "paused", "archived"];

  const filteredProjects = $derived(selectedFilter === 'all'
    ? projects
    : projects.filter(p => p.status === selectedFilter));

  function toggleTheme() {
    theme.update(t => {
      const next = t === 'dark' ? 'light' : 'dark';
      document.documentElement.setAttribute('data-theme', next === 'light' ? 'light' : '');
      return next;
    });
  }

  // Typed intro
  const intro = "developer. builder. tinkerer.";
  let typedText = $state('');
  let typingDone = $state(false);

  $effect(() => {
    let i = 0;
    const t = setInterval(() => {
      typedText = intro.slice(0, i + 1);
      i++;
      if (i >= intro.length) {
        clearInterval(t);
        typingDone = true;
      }
    }, 45);
    return () => clearInterval(t);
  });
</script>

<svelte:head>
  <title>duanleks.space</title>
  <meta name="description" content="DuanLeks — personal space. Tools, experiments, and projects." />
  <meta name="robots" content="index, follow" />
  <link rel="canonical" href="https://duanleks.space/" />
  <meta property="og:title" content="duanleks.space" />
  <meta property="og:description" content="DuanLeks — personal space. Tools, experiments, and projects." />
  <meta property="og:url" content="https://duanleks.space/" />
  <meta property="og:type" content="website" />
  {@html `<script type="application/ld+json">${JSON.stringify({
    "@context": "https://schema.org",
    "@type": "Person",
    "name": "DuanLeks",
    "url": "https://duanleks.space",
    "sameAs": ["https://github.com/NandaIda"],
    "mainEntityOfPage": { "@type": "WebPage", "@id": "https://duanleks.space/" }
  })}</script>`}
</svelte:head>

<main>
  <div class="bg-blob blob-1" aria-hidden="true"></div>
  <div class="bg-blob blob-2" aria-hidden="true"></div>

  <div class="page-inner">

    <!-- ── Hero ── -->
    <header class="hero">
      <div class="hero-top-row">
        <div class="hero-eyebrow">
          <span class="prompt">~/duanleks</span>
          <span class="prompt-sep"> $ </span>
          <span class="cmd">whoami</span>
        </div>

        <!-- Theme toggle -->
        <button class="theme-toggle" onclick={toggleTheme} aria-label="Toggle theme"
                title={$theme === 'dark' ? 'Switch to light theme' : 'Switch to dark theme'}>
          {#if $theme === 'dark'}
            <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <circle cx="12" cy="12" r="5"/>
              <line x1="12" y1="1" x2="12" y2="3"/>
              <line x1="12" y1="21" x2="12" y2="23"/>
              <line x1="4.22" y1="4.22" x2="5.64" y2="5.64"/>
              <line x1="18.36" y1="18.36" x2="19.78" y2="19.78"/>
              <line x1="1" y1="12" x2="3" y2="12"/>
              <line x1="21" y1="12" x2="23" y2="12"/>
              <line x1="4.22" y1="19.78" x2="5.64" y2="18.36"/>
              <line x1="18.36" y1="5.64" x2="19.78" y2="4.22"/>
            </svg>
          {:else}
            <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <path d="M21 12.79A9 9 0 1 1 11.21 3 7 7 0 0 0 21 12.79z"/>
            </svg>
          {/if}
          <span class="toggle-label">{$theme === 'dark' ? 'light' : 'dark'}</span>
        </button>
      </div>

      <h1 class="hero-name">DuanLeks</h1>

      <div class="hero-typed" aria-label={intro}>
        <span>{typedText}</span>
        {#if !typingDone}
          <span class="cursor-blink">▌</span>
        {/if}
      </div>

      <p class="hero-sub">
        I build things to scratch my own itches — mostly tools I wish existed.
        Most of these started as weekend experiments that got out of hand.
      </p>

      <div class="hero-divider"></div>
    </header>

    <!-- ── Filters ── -->
    <nav class="filters" aria-label="Filter projects by status">
      <span class="filter-label">filter:</span>
      <div class="filter-buttons">
        {#each filters as filter}
          <button
            onclick={() => selectedFilter = filter}
            class="filter-btn"
            class:active={selectedFilter === filter}
            aria-pressed={selectedFilter === filter}>
            {filter}
          </button>
        {/each}
      </div>
    </nav>

    <!-- ── Grid ── -->
    <section class="grid-section" aria-label="Projects">
      {#each filteredProjects as project, i}
        <div class="fade-up" style="animation-delay: {i * 50}ms">
          <ProjectCard {project} />
        </div>
      {/each}
    </section>

    <!-- ── Footer ── -->
    <footer class="site-footer">
      <span class="footer-prompt">// more projects migrating from vercel — check back soon</span>
    </footer>

  </div>
</main>

<style>
  main {
    min-height: 100vh;
    position: relative;
    overflow: hidden;
  }

  .bg-blob {
    position: fixed;
    border-radius: 50%;
    filter: blur(120px);
    pointer-events: none;
    z-index: 0;
  }

  .blob-1 {
    width: 500px;
    height: 500px;
    background: radial-gradient(circle, var(--blob-1) 0%, transparent 70%);
    top: -100px;
    right: -100px;
  }

  .blob-2 {
    width: 600px;
    height: 400px;
    background: radial-gradient(circle, var(--blob-2) 0%, transparent 70%);
    bottom: 100px;
    left: -150px;
  }

  @media (max-width: 640px) {
    .bg-blob {
      display: none;
    }
  }

  .page-inner {
    position: relative;
    z-index: 1;
    max-width: 1120px;
    margin: 0 auto;
    padding: 72px 32px 80px;
  }

  /* ── Hero ── */
  .hero { margin-bottom: 56px; }

  .hero-top-row {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 20px;
  }

  .hero-eyebrow {
    font-family: 'JetBrains Mono', monospace;
    font-size: 14px;
    color: var(--text-muted);
    display: flex;
    align-items: center;
  }

  .prompt { color: var(--prompt-color); }
  .prompt-sep { color: var(--text-muted); }
  .cmd { color: var(--accent); }

  /* ── Theme toggle ── */
  .theme-toggle {
    display: flex;
    align-items: center;
    gap: 6px;
    font-family: 'JetBrains Mono', monospace;
    font-size: 13px;
    font-weight: 500;
    letter-spacing: 0.05em;
    color: var(--text-muted);
    background: var(--tag-bg);
    border: 1px solid var(--filter-border);
    border-radius: 6px;
    padding: 8px 14px;
    cursor: pointer;
    transition: color 0.2s, border-color 0.2s, background 0.2s;
  }

  .theme-toggle:hover {
    color: var(--text-secondary);
    border-color: var(--border-hover);
    background: color-mix(in srgb, var(--accent) 8%, transparent);
  }

  .toggle-label { text-transform: lowercase; }

  .hero-name {
    font-family: 'Syne', sans-serif;
    font-size: clamp(36px, 8vw, 88px);
    font-weight: 800;
    letter-spacing: -0.03em;
    color: var(--text-primary);
    line-height: 1;
    margin: 0 0 12px;
  }

  .hero-typed {
    font-family: 'JetBrains Mono', monospace;
    font-size: clamp(16px, 2.5vw, 20px);
    color: var(--text-secondary);
    letter-spacing: 0.02em;
    margin-bottom: 20px;
    height: 1.4em;
    display: flex;
    align-items: center;
    gap: 1px;
  }

  .hero-sub {
    font-family: 'JetBrains Mono', monospace;
    font-size: 15px;
    color: var(--text-secondary);
    line-height: 1.8;
    max-width: 560px;
    margin: 0;
  }

  .hero-divider {
    margin-top: 40px;
    height: 1px;
    background: linear-gradient(90deg, var(--divider) 0%, transparent 60%);
  }

  /* ── Filters ── */
  .filters {
    display: flex;
    align-items: center;
    gap: 16px;
    margin-bottom: 32px;
    flex-wrap: wrap;
  }

  .filter-label {
    font-family: 'JetBrains Mono', monospace;
    font-size: 13px;
    color: var(--text-muted);
    letter-spacing: 0.05em;
    flex-shrink: 0;
  }

  .filter-buttons {
    display: flex;
    gap: 8px;
    flex-wrap: wrap;
  }

  .filter-btn {
    font-family: 'JetBrains Mono', monospace;
    font-size: 13px;
    font-weight: 500;
    letter-spacing: 0.04em;
    padding: 7px 16px;
    border-radius: 6px;
    border: 1px solid var(--filter-border);
    background: transparent;
    color: var(--tag-color);
    cursor: pointer;
    transition: all 0.15s ease;
    /* Larger tap target on mobile */
    min-height: 36px;
  }

  .filter-btn:hover {
    border-color: var(--border-hover);
    color: var(--text-primary);
    background: var(--tag-bg);
  }

  .filter-btn.active {
    background: var(--accent);
    border-color: var(--accent);
    color: #0a0c0f;
    font-weight: 600;
  }

  /* ── Grid ── */
  .grid-section {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
    gap: 16px;
  }

  /* ── Footer ── */
  .site-footer {
    margin-top: 64px;
    padding-top: 24px;
    border-top: 1px solid var(--divider);
  }

  .footer-prompt {
    font-family: 'JetBrains Mono', monospace;
    font-size: 13px;
    color: var(--text-muted);
    opacity: 0.6;
    letter-spacing: 0.03em;
  }

  /* ── Mobile ── */
  @media (max-width: 640px) {
    .page-inner {
      padding: 32px 16px 56px;
    }

    .hero {
      margin-bottom: 40px;
    }

    .hero-top-row {
      margin-bottom: 16px;
    }

    .hero-eyebrow {
      font-size: 12px;
    }

    .theme-toggle {
      font-size: 12px;
      padding: 6px 10px;
      gap: 5px;
    }

    .toggle-label {
      display: none;
    }

    .hero-sub {
      font-size: 14px;
    }

    .filters {
      flex-direction: column;
      align-items: flex-start;
      gap: 10px;
      margin-bottom: 24px;
    }

    .filter-buttons {
      gap: 6px;
    }

    .filter-btn {
      font-size: 12px;
      padding: 6px 14px;
    }

    .grid-section {
      grid-template-columns: 1fr;
      gap: 12px;
    }
  }

  /* ── Tablet ── */
  @media (min-width: 641px) and (max-width: 900px) {
    .page-inner {
      padding: 56px 24px 72px;
    }

    .grid-section {
      grid-template-columns: repeat(2, 1fr);
    }
  }
</style>
