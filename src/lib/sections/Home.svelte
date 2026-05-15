<script lang="ts">
	import { onMount } from 'svelte';
	import Scene from '$lib/three/Scene.svelte';
	import gsap from 'gsap';

	let sectionRef: HTMLElement;
	let eyebrowRef: HTMLElement;
	let headingRef: HTMLElement;
	let descRef: HTMLElement;
	let scrollRef: HTMLElement;
	let sceneRef: HTMLElement;

	onMount(() => {
		// Set initial states
		gsap.set([eyebrowRef, headingRef, descRef, scrollRef], { opacity: 0 });
		gsap.set(eyebrowRef, { y: 20 });
		gsap.set(headingRef, { y: 50 });
		gsap.set(descRef, { y: 30 });
		gsap.set(scrollRef, { y: 16, opacity: 0 });
		gsap.set(sceneRef, { opacity: 0, x: 40 });

		const tl = gsap.timeline({ defaults: { ease: 'power3.out' } });

		tl.to(eyebrowRef, { y: 0, opacity: 1, duration: 0.7 }, 0.3)
			.to(headingRef, { y: 0, opacity: 1, duration: 0.95 }, 0.5)
			.to(descRef, { y: 0, opacity: 1, duration: 0.8 }, 0.8)
			.to(sceneRef, { x: 0, opacity: 1, duration: 1.1, ease: 'power2.out' }, 0.2)
			.to(scrollRef, { y: 0, opacity: 1, duration: 0.7 }, 1.3);
	});

	function scrollDown() {
		window.scrollBy({ top: window.innerHeight, behavior: 'smooth' });
	}
</script>

<section class="hero" bind:this={sectionRef}>
	<!-- Background atmosphere -->
	<div class="bg-base"></div>
	<div class="bg-blob blob-tl"></div>
	<div class="bg-blob blob-br"></div>
	<div class="bg-grid"></div>
	<div class="grain"></div>

	<!-- Desktop layout: text left, scene right -->
	<!-- Mobile layout: scene fills bg, text center overlay -->
	<div class="hero-layout">
		<!-- ── LEFT: Text content ── -->
		<div class="text-col">
			<span class="eyebrow" bind:this={eyebrowRef}>
				<span class="eyebrow-dot"></span>
				Frontend Engineer & Interaction Designer
			</span>

			<h1 class="heading" bind:this={headingRef}>
				Engineering
				<br />
				<em class="heading-accent">interfaces</em>
				<br />
				that feel
				<br />
				<span class="heading-whisper">alive.</span>
			</h1>

			<p class="desc" bind:this={descRef}>
				I sit at the intersection of engineering precision and design sensibility — crafting
				immersive digital experiences with motion, depth, and intentional interaction.
			</p>

			<!-- Subtle availability line -->
			<div class="avail-line" bind:this={descRef}>
				<span class="avail-dot"></span>
				Open to opportunities · Based in Nigeria
			</div>
		</div>

		<!-- ── RIGHT: 3D Scene ── -->
		<div class="scene-col" bind:this={sceneRef}>
			<Scene height="100%" />
		</div>
	</div>

	<!-- ── Mobile scene background (behind text) ── -->
	<div class="scene-mobile-bg">
		<Scene height="100%" />
		<div class="mobile-shadow-overlay"></div>
	</div>

	<!-- ── Center scroll indicator ── -->
	<button
		class="scroll-btn"
		bind:this={scrollRef}
		on:click={scrollDown}
		aria-label="Scroll to next section"
	>
		<div class="scroll-track">
			<div class="scroll-nub"></div>
		</div>
		<span class="scroll-label">Scroll</span>
	</button>
</section>

<style>
	/* ── Palette ── */
	:root {
		--c-dark: #1f2421;
		--c-teal: #216869;
		--c-green: #49a078;
		--c-sage: #9cc5a1;
		--c-mist: #dce1de;
		--c-bg: #0a0e0c;
	}

	/* ── Shell ── */
	.hero {
		position: relative;
		width: 100%;
		min-height: 100svh;
		overflow: hidden;
		font-family: 'DM Sans', 'Outfit', system-ui, sans-serif;
		background: var(--c-bg);
		isolation: isolate;
	}

	/* ── Background layers ── */
	.bg-base {
		position: absolute;
		inset: 0;
		background: radial-gradient(ellipse at 30% 50%, #0d1a14 0%, #0a0e0c 60%);
		z-index: 0;
	}
	.bg-blob {
		position: absolute;
		border-radius: 50%;
		filter: blur(110px);
		pointer-events: none;
		z-index: 1;
	}
	.blob-tl {
		width: 700px;
		height: 600px;
		top: -150px;
		left: -100px;
		background: radial-gradient(ellipse, rgba(33, 104, 105, 0.22) 0%, transparent 70%);
	}
	.blob-br {
		width: 500px;
		height: 500px;
		bottom: -80px;
		right: -80px;
		background: radial-gradient(ellipse, rgba(73, 160, 120, 0.14) 0%, transparent 70%);
	}

	/* Subtle dot grid */
	.bg-grid {
		position: absolute;
		inset: 0;
		z-index: 2;
		pointer-events: none;
		opacity: 0.025;
		background-image: radial-gradient(circle, rgba(220, 225, 222, 0.8) 1px, transparent 1px);
		background-size: 36px 36px;
	}

	/* Grain */
	.grain {
		position: absolute;
		inset: 0;
		z-index: 3;
		pointer-events: none;
		opacity: 0.038;
		background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
		background-repeat: repeat;
		background-size: 200px;
	}

	/* ── Hero layout — desktop ── */
	.hero-layout {
		position: relative;
		z-index: 10;
		max-width: 1380px;
		margin: 0 auto;
		min-height: 100svh;
		display: grid;
		grid-template-columns: 0.8fr 1.45fr;
		align-items: center;
		padding: 0 4rem;
		gap: 0;
	}

	/* ── Mobile scene background ── */
	.scene-mobile-bg {
		display: none; /* hidden on desktop */
		position: absolute;
		inset: 0;
		z-index: 4;
	}
	.mobile-shadow-overlay {
		position: absolute;
		inset: 0;
		/* Heavy center shadow so text pops over the 3D character */
		background:
			radial-gradient(ellipse at 50% 60%, rgba(10, 14, 12, 0.15) 0%, rgba(10, 14, 12, 0.82) 65%),
			linear-gradient(to top, rgba(10, 14, 12, 0.95) 0%, rgba(10, 14, 12, 0.4) 60%);
		z-index: 2;
	}

	/* ── Text column ── */
	.text-col {
		display: flex;
		flex-direction: column;
		gap: 1.5rem;
		max-width: 560px;
		padding: 6rem 0;
	}

	.eyebrow {
		display: inline-flex;
		align-items: center;
		gap: 0.6rem;
		font-size: 0.65rem;
		font-weight: 700;
		letter-spacing: 0.28em;
		text-transform: uppercase;
		color: var(--c-sage);
		opacity: 0.8;
	}
	.eyebrow-dot {
		flex-shrink: 0;
		width: 6px;
		height: 6px;
		border-radius: 50%;
		background: var(--c-green);
		box-shadow: 0 0 0 3px rgba(73, 160, 120, 0.22);
		animation: pulse-dot 2.6s ease-in-out infinite;
	}
	@keyframes pulse-dot {
		0%,
		100% {
			box-shadow: 0 0 0 3px rgba(73, 160, 120, 0.22);
		}
		50% {
			box-shadow: 0 0 0 8px rgba(73, 160, 120, 0.06);
		}
	}

	/* Heading */
	.heading {
		font-size: clamp(3rem, 5.5vw, 5.2rem);
		font-weight: 900;
		letter-spacing: -0.05em;
		line-height: 1.02;
		color: var(--c-mist);
		margin: 0;
	}
	.heading-accent {
		font-style: normal;
		background: linear-gradient(100deg, var(--c-sage), var(--c-green));
		-webkit-background-clip: text;
		-webkit-text-fill-color: transparent;
		background-clip: text;
	}
	.heading-whisper {
		color: rgba(220, 225, 222, 0.35);
		position: relative;
	}
	/* Underline on "alive." */
	.heading-whisper::after {
		content: '';
		position: absolute;
		bottom: 4px;
		left: 0;
		right: 0;
		height: 2px;
		background: linear-gradient(90deg, var(--c-green), transparent);
		border-radius: 2px;
		opacity: 0.5;
	}

	/* Description */
	.desc {
		font-size: 1rem;
		line-height: 1.78;
		color: rgba(220, 225, 222, 0.48);
		max-width: 46ch;
		margin: 0;
	}

	/* Availability line */
	.avail-line {
		display: inline-flex;
		align-items: center;
		gap: 0.55rem;
		font-size: 0.68rem;
		font-weight: 600;
		letter-spacing: 0.1em;
		color: rgba(220, 225, 222, 0.3);
		margin-top: 0.5rem;
	}
	.avail-dot {
		width: 5px;
		height: 5px;
		border-radius: 50%;
		background: var(--c-green);
		flex-shrink: 0;
	}

	/* ── Scene column — desktop ── */
	.scene-col {
		height: 100svh;
		position: relative;
		margin-left: -3rem;
	}

	/* ── Scroll button ── */
	.scroll-btn {
		position: absolute;
		bottom: 2.5rem;
		left: 50%;
		transform: translateX(-50%);
		z-index: 20;
		display: flex;
		flex-direction: column;
		align-items: center;
		gap: 0.55rem;
		background: none;
		border: none;
		cursor: pointer;
		padding: 0;
	}

	.scroll-track {
		width: 26px;
		height: 44px;
		border-radius: 13px;
		border: 1.5px solid rgba(156, 197, 161, 0.35);
		position: relative;
		display: flex;
		justify-content: center;
		padding-top: 7px;
		background: rgba(73, 160, 120, 0.06);
		/* Outer pulse ring */
		animation: track-pulse 2.8s ease-in-out infinite;
	}
	@keyframes track-pulse {
		0%,
		100% {
			box-shadow: 0 0 0 0 rgba(73, 160, 120, 0);
		}
		50% {
			box-shadow: 0 0 0 8px rgba(73, 160, 120, 0.08);
		}
	}

	.scroll-nub {
		width: 5px;
		height: 5px;
		border-radius: 50%;
		background: var(--c-sage);
		animation: nub-drop 2.2s cubic-bezier(0.45, 0, 0.55, 1) infinite;
	}
	@keyframes nub-drop {
		0% {
			transform: translateY(0);
			opacity: 1;
		}
		60% {
			transform: translateY(18px);
			opacity: 0.2;
		}
		61% {
			transform: translateY(0);
			opacity: 0;
		}
		70% {
			opacity: 1;
		}
		100% {
			transform: translateY(0);
			opacity: 1;
		}
	}

	.scroll-label {
		font-size: 0.58rem;
		font-weight: 700;
		letter-spacing: 0.3em;
		text-transform: uppercase;
		color: rgba(156, 197, 161, 0.45);
	}

	/* ═══════════════════════
	   MOBILE  (≤ 768px)
	═══════════════════════ */
	@media (max-width: 768px) {
		.hero-layout {
			grid-template-columns: 1fr;
			grid-template-rows: 1fr;
			padding: 0 1.75rem;
			z-index: 10;
		}

		/* Hide the desktop scene col */
		.scene-col {
			display: none;
		}

		/* Show mobile bg scene */
		.scene-mobile-bg {
			display: block;
		}

		/* Text col sits on top of the bg scene */
		.text-col {
			position: relative;
			z-index: 12;
			text-align: center;
			align-items: center;
			max-width: 100%;
			min-height: 100svh;
			justify-content: center;
			padding: 6rem 0 8rem;
		}

		.heading {
			font-size: clamp(2.6rem, 10vw, 4rem);
		}
		.desc {
			text-align: center;
			color: rgba(220, 225, 222, 0.7);
		}
		.eyebrow {
			text-align: center;
		}
	}

	@media (min-width: 769px) and (max-width: 1100px) {
		.hero-layout {
			padding: 0 2.5rem;
		}
		.heading {
			font-size: clamp(2.8rem, 4.5vw, 4.2rem);
		}
	}
</style>
