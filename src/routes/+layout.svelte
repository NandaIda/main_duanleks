<script>
  import "../app.css";
  import favicon from '$lib/assets/favicon.svg';
  let { children } = $props();
</script>

<svelte:head>
  <link rel="icon" type="image/svg+xml" href="{favicon}" />
  <link rel="apple-touch-icon" href="{favicon}" />
  <meta name="theme-color" content="#0a0c0f" />
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin="anonymous" />
  <link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&family=JetBrains+Mono:wght@300;400;500;600&display=swap" rel="stylesheet" />
</svelte:head>

{@render children()}
