<script>
  import StatusBadge from './StatusBadge.svelte';
  import ProjectIcon from './ProjectIcon.svelte';
  import { theme } from '$lib/theme.js';
  let { project } = $props();

  const darkLogoMap = {
    mindmapp: { dark: '/logos/mindmap_white.png', light: '/logos/mindmap.png' },
    mermaid: { dark: '/logos/mermaid_white.svg', light: '/logos/mermaid.svg' },
  };

  let logoSrc = $derived(
    darkLogoMap[project.name]
      ? darkLogoMap[project.name][$theme]
      : project.logo
  );
</script>

<a href={project.url} target="_blank" rel="noopener noreferrer"
   class="project-card"
   style="--accent: {project.color}">

  <div class="card-header">
    {#if project.logo}
      <img src={logoSrc} alt="{project.name} logo"
           class="project-logo" />
    {:else}
      <ProjectIcon name={project.name} color={project.color} />
    {/if}
    <StatusBadge status={project.status} />
  </div>

  <div class="card-body">
    <h2 class="project-name">{project.name}</h2>
    <p class="project-desc">{project.description}</p>
    {#if project.story}
      <p class="project-story">// {project.story}</p>
    {/if}
  </div>

  <div class="card-footer">
    <div class="tags">
      {#each project.tags as tag}
        <span class="tag">{tag}</span>
      {/each}
    </div>
    <span class="arrow">→</span>
  </div>

  <div class="card-glow"></div>
</a>

<style>
  .project-card {
    position: relative;
    display: flex;
    flex-direction: column;
    background: var(--bg-card);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 20px;
    text-decoration: none;
    color: inherit;
    overflow: hidden;
    transition: border-color 0.25s ease, transform 0.2s ease, background 0.25s ease;
    cursor: pointer;
  }

  .project-card:hover {
    border-color: color-mix(in srgb, var(--accent) 40%, transparent);
    background: var(--bg-card-hover);
    transform: translateY(-2px);
  }

  .project-card:hover .card-glow { opacity: 1; }
  .project-card:hover .arrow { opacity: 1; transform: translateX(2px); }

  .card-glow {
    position: absolute;
    inset: 0;
    border-radius: 12px;
    background: radial-gradient(
      ellipse at 50% 0%,
      color-mix(in srgb, var(--accent) 10%, transparent) 0%,
      transparent 70%
    );
    opacity: 0;
    transition: opacity 0.3s ease;
    pointer-events: none;
  }

  .card-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 16px;
  }

  .project-logo {
    width: 40px;
    height: 40px;
    object-fit: contain;
    border-radius: 8px;
    transition: filter 0.3s ease;
  }

  .card-body {
    flex: 1;
    display: flex;
    flex-direction: column;
    gap: 6px;
    margin-bottom: 16px;
  }

  .project-name {
    font-family: 'Syne', sans-serif;
    font-size: 20px;
    font-weight: 700;
    color: var(--text-primary);
    letter-spacing: -0.01em;
    margin: 0;
  }

  .project-desc {
    font-family: 'JetBrains Mono', monospace;
    font-size: 14px;
    color: var(--text-primary);
    line-height: 1.6;
    margin: 0;
  }

  .project-story {
    font-family: 'JetBrains Mono', monospace;
    font-size: 13px;
    color: var(--text-secondary);
    line-height: 1.6;
    margin: 6px 0 0;
  }

  .card-footer {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 8px;
  }

  .tags {
    display: flex;
    gap: 6px;
    flex-wrap: wrap;
  }

  .tag {
    font-family: 'JetBrains Mono', monospace;
    font-size: 12px;
    font-weight: 500;
    letter-spacing: 0.04em;
    padding: 4px 10px;
    border-radius: 4px;
    background: var(--tag-bg);
    color: var(--tag-color);
    border: 1px solid var(--tag-border);
    transition: color 0.2s, background 0.2s;
  }

  .project-card:hover .tag {
    color: color-mix(in srgb, var(--accent) 80%, var(--text-secondary));
    background: color-mix(in srgb, var(--accent) 8%, transparent);
    border-color: color-mix(in srgb, var(--accent) 20%, transparent);
  }

  .arrow {
    font-size: 18px;
    color: var(--accent);
    opacity: 0;
    transition: opacity 0.2s, transform 0.2s;
    flex-shrink: 0;
  }

  @media (max-width: 640px) {
    .project-card {
      padding: 18px;
    }

    .project-logo,
    .logo-placeholder {
      width: 44px;
      height: 44px;
    }

    .project-name {
      font-size: 19px;
    }

    .project-desc {
      font-size: 14px;
    }

    /* Always show arrow on mobile (no hover) */
    .arrow {
      opacity: 0.5;
    }
  }
</style>
