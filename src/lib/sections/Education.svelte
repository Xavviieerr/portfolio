<script lang="ts">
	import { onMount } from 'svelte';
	import gsap from 'gsap';

	let container: HTMLDivElement;
	let headerRef: HTMLDivElement;

	onMount(() => {
		// Kill any existing tweens to prevent conflicts
		gsap.killTweensOf('*');

		const cards = container?.querySelectorAll('[data-card]');
		if (!cards || cards.length === 0) return;

		// Set initial state explicitly before animating
		gsap.set(cards, { y: 60, opacity: 0, scale: 0.96 });
		gsap.set(headerRef, { y: 24, opacity: 0 });

		const tl = gsap.timeline({ defaults: { ease: 'power3.out' } });

		tl.to(headerRef, { y: 0, opacity: 1, duration: 0.7 }).to(
			cards,
			{
				y: 0,
				opacity: 1,
				scale: 1,
				duration: 0.75,
				stagger: { amount: 0.55, from: 'start' }
			},
			'-=0.35'
		);
	});

	const education = [
		{
			title: 'BSc Computer Science',
			school: 'University Name',
			year: '2022 — 2026',
			description: 'Focused on frontend engineering, software systems, and interaction design.',
			index: '01'
		},
		{
			title: 'Frontend Engineering',
			school: 'Self Directed Learning',
			year: '2023 — Present',
			description: 'Studying immersive interfaces, animation systems, and modern UI architecture.',
			index: '02'
		},
		{
			title: 'Backend Foundations',
			school: 'Independent Study',
			year: '2023',
			description:
				'Worked with APIs, databases, authentication systems, and application structure.',
			index: '03'
		},
		{
			title: 'Creative Development',
			school: 'Interactive Web Exploration',
			year: '2024',
			description: 'Exploring cinematic interfaces, motion systems, and interactive storytelling.',
			index: '04'
		},
		{
			title: 'AI + Product Thinking',
			school: 'Personal Research',
			year: '2025',
			description:
				'Researching intelligent systems, user experience, and product-driven engineering.',
			index: '05'
		}
	];
</script>

<section class="edu-section">
	<!-- Ambient background shapes -->
	<div class="ambient-blob blob-1"></div>
	<div class="ambient-blob blob-2"></div>
	<div class="ambient-blob blob-3"></div>

	<!-- Grain overlay -->
	<div class="grain"></div>

	<!-- SECTION HEADER -->
	<div class="header-wrap" bind:this={headerRef}>
		<span class="label-eyebrow">Selected Background</span>
		<h2 class="section-title">Education</h2>
		<div class="title-rule"></div>
	</div>

	<!-- CARD GRID -->
	<div bind:this={container} class="card-grid">
		{#each education as item, i}
			<div class="card-col" class:offset={i % 2 !== 0}>
				<article data-card class="card">
					<!-- Top accent line -->
					<div class="card-accent"></div>

					<div class="card-inner">
						<div class="card-meta">
							<span class="card-index">{item.index}</span>
							<span class="card-year">{item.year}</span>
						</div>

						<h3 class="card-title">{item.title}</h3>
						<p class="card-school">{item.school}</p>

						<div class="card-divider"></div>

						<p class="card-desc">{item.description}</p>

						<!-- Decorative corner dot -->
						<div class="card-dot"></div>
					</div>
				</article>
			</div>
		{/each}
	</div>
</section>

<style>
	/* ── Palette ── */
	:root {
		--c-dark: #1f2421;
		--c-teal: #216869;
		--c-green: #49a078;
		--c-sage: #9cc5a1;
		--c-mist: #dce1de;
		--c-bg: #f4f6f4;
		--c-surface: rgba(255, 255, 255, 0.72);
		--c-border: rgba(73, 160, 120, 0.18);
		--c-shadow: rgba(31, 36, 33, 0.08);
	}

	/* ── Section shell ── */
	.edu-section {
		position: relative;
		min-height: 100svh;
		overflow: hidden;
		background: var(--c-bg);
		padding: 7rem 1.5rem 8rem;
		font-family: 'DM Sans', 'Outfit', system-ui, sans-serif;
	}

	/* ── Ambient blobs ── */
	.ambient-blob {
		position: absolute;
		border-radius: 50%;
		filter: blur(80px);
		pointer-events: none;
		z-index: 0;
	}
	.blob-1 {
		width: 700px;
		height: 500px;
		top: -100px;
		left: -180px;
		background: radial-gradient(ellipse, rgba(156, 197, 161, 0.28) 0%, transparent 70%);
	}
	.blob-2 {
		width: 500px;
		height: 400px;
		top: 30%;
		right: -120px;
		background: radial-gradient(ellipse, rgba(33, 104, 105, 0.18) 0%, transparent 70%);
	}
	.blob-3 {
		width: 400px;
		height: 400px;
		bottom: 0;
		left: 30%;
		background: radial-gradient(ellipse, rgba(73, 160, 120, 0.13) 0%, transparent 70%);
	}

	/* ── Grain ── */
	.grain {
		position: absolute;
		inset: 0;
		z-index: 1;
		pointer-events: none;
		opacity: 0.022;
		background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)'/%3E%3C/svg%3E");
		background-repeat: repeat;
		background-size: 200px;
	}

	/* ── Header ── */
	.header-wrap {
		position: relative;
		z-index: 2;
		text-align: center;
		margin-bottom: 5rem;
	}
	.label-eyebrow {
		display: inline-block;
		font-size: 0.65rem;
		font-weight: 600;
		letter-spacing: 0.28em;
		text-transform: uppercase;
		color: var(--c-teal);
		margin-bottom: 0.9rem;
	}
	.section-title {
		font-size: clamp(2.4rem, 5vw, 3.8rem);
		font-weight: 700;
		letter-spacing: -0.03em;
		color: var(--c-dark);
		line-height: 1;
		margin: 0 0 1.2rem;
	}
	.title-rule {
		width: 40px;
		height: 3px;
		background: linear-gradient(90deg, var(--c-green), var(--c-sage));
		border-radius: 2px;
		margin: 0 auto;
	}

	/* ── Grid ── */
	.card-grid {
		position: relative;
		z-index: 2;
		display: grid;
		grid-template-columns: repeat(1, 1fr);
		gap: 1.25rem;
		max-width: 1280px;
		margin: 0 auto;
	}
	@media (min-width: 768px) {
		.card-grid {
			grid-template-columns: repeat(5, 1fr);
			gap: 1rem;
		}
		.card-col.offset {
			margin-top: 5rem;
		}
	}

	/* ── Card ── */
	.card {
		position: relative;
		border-radius: 24px;
		background: var(--c-surface);
		border: 1px solid var(--c-border);
		box-shadow:
			0 2px 0 rgba(255, 255, 255, 0.9) inset,
			0 12px 40px var(--c-shadow);
		backdrop-filter: blur(20px) saturate(1.4);
		-webkit-backdrop-filter: blur(20px) saturate(1.4);
		overflow: hidden;
		transition:
			transform 0.4s cubic-bezier(0.34, 1.56, 0.64, 1),
			box-shadow 0.4s ease,
			background 0.4s ease;
		cursor: default;
	}
	.card:hover {
		transform: translateY(-10px) scale(1.015);
		box-shadow:
			0 2px 0 rgba(255, 255, 255, 0.95) inset,
			0 24px 60px rgba(31, 36, 33, 0.12),
			0 0 0 1px rgba(73, 160, 120, 0.25);
		background: rgba(255, 255, 255, 0.85);
	}

	/* Hover — reveal accent line */
	.card-accent {
		position: absolute;
		top: 0;
		left: 0;
		right: 0;
		height: 3px;
		background: linear-gradient(90deg, var(--c-teal), var(--c-green), var(--c-sage));
		opacity: 0;
		transform: scaleX(0.6);
		transform-origin: left;
		transition:
			opacity 0.35s ease,
			transform 0.35s ease;
	}
	.card:hover .card-accent {
		opacity: 1;
		transform: scaleX(1);
	}

	/* Card inner padding */
	.card-inner {
		padding: 1.5rem;
		position: relative;
	}

	/* Meta row */
	.card-meta {
		display: flex;
		align-items: center;
		justify-content: space-between;
		margin-bottom: 1.1rem;
	}
	.card-index {
		font-size: 0.65rem;
		font-weight: 700;
		letter-spacing: 0.12em;
		color: var(--c-green);
		font-variant-numeric: tabular-nums;
	}
	.card-year {
		font-size: 0.62rem;
		font-weight: 500;
		letter-spacing: 0.14em;
		text-transform: uppercase;
		color: #7a8c82;
	}

	/* Title */
	.card-title {
		font-size: 1rem;
		font-weight: 700;
		letter-spacing: -0.02em;
		line-height: 1.25;
		color: var(--c-dark);
		margin: 0 0 0.35rem;
	}

	/* School */
	.card-school {
		font-size: 0.72rem;
		font-weight: 500;
		color: var(--c-teal);
		margin: 0;
		letter-spacing: 0.01em;
	}

	/* Divider */
	.card-divider {
		width: 28px;
		height: 1.5px;
		background: linear-gradient(90deg, var(--c-sage), transparent);
		margin: 1rem 0;
		border-radius: 1px;
	}

	/* Description */
	.card-desc {
		font-size: 0.78rem;
		line-height: 1.65;
		color: #4a5550;
		margin: 0;
	}

	/* Decorative corner dot */
	.card-dot {
		position: absolute;
		bottom: 1.2rem;
		right: 1.2rem;
		width: 6px;
		height: 6px;
		border-radius: 50%;
		background: var(--c-mist);
		border: 1.5px solid var(--c-sage);
		opacity: 0.7;
		transition: opacity 0.3s ease;
	}
	.card:hover .card-dot {
		opacity: 1;
		background: var(--c-sage);
	}
</style>
