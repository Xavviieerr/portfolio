<script lang="ts">
	import './layout.css';
	import favicon from '$lib/assets/favicon.svg';

	let { children } = $props();
</script>

<svelte:head>
	<!-- Identity -->
	<title>Chidera Paul Ogbu — Frontend Engineer & Interaction Designer</title>
	<meta name="description" content="Portfolio of Chidera Paul Ogbu — a frontend engineer and interaction designer crafting immersive digital experiences with motion, depth, and intentional interaction." />
	<meta name="author" content="Chidera Paul Ogbu" />
	<meta name="keywords" content="frontend engineer, interaction designer, SvelteKit, Three.js, UI design, motion design, Nigeria, portfolio" />

	<!-- Favicon -->
	<link rel="icon" type="image/svg+xml" href={favicon} />

	<!-- Open Graph (social sharing) -->
	<meta property="og:type" content="website" />
	<meta property="og:title" content="Chidera Paul Ogbu — Frontend Engineer" />
	<meta property="og:description" content="Engineering interfaces that feel alive — interactive 3D portfolio built with SvelteKit and Three.js." />
	<meta property="og:image" content="/og-image.png" />
	<meta property="og:url" content="https://cpogbu.dev" />
	<meta property="og:site_name" content="CPO — Portfolio" />

	<!-- Twitter / X card -->
	<meta name="twitter:card" content="summary_large_image" />
	<meta name="twitter:title" content="Chidera Paul Ogbu — Frontend Engineer" />
	<meta name="twitter:description" content="Engineering interfaces that feel alive." />
	<meta name="twitter:image" content="/og-image.png" />

	<!-- Misc -->
	<meta name="theme-color" content="#0a0e0c" />
	<meta name="color-scheme" content="dark" />
	<meta name="viewport" content="width=device-width, initial-scale=1" />
</svelte:head>

{@render children()}
