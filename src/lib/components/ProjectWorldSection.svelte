<script lang="ts">
	import { onMount, tick } from 'svelte';
	import gsap from 'gsap';

	// ── Types ──────────────────────────────────────────────────────────────
	type Artifact = {
		id: number;
		shape: 'circle' | 'ring' | 'pill' | 'square' | 'leaf' | 'cookie' | 'lock' | 'blob';
		size: number;
		top: string;
		left: string;
		dur: number;
		delay: number;
		color: string;
		rotate?: number;
		blur?: number;
		imgSrc?: string; // swap with real asset
	};

	type Project = {
		id: number;
		index: string;
		title: string;
		tagline: string;
		description: string;
		metrics: { label: string; value: string }[];
		stack: string[];
		live: string;
		github: string;
		bg: string;
		accentColor: string;
		cardBorder: string;
		artifacts: Artifact[];
	};

	// ── Project Data ───────────────────────────────────────────────────────
	const projects: Project[] = [
		{
			id: 1,
			index: '01',
			title: 'PageSnack',
			tagline: 'AI-Powered Chrome Extension',
			description:
				'A premium Chrome Extension delivering instant, structured AI summaries of any webpage through a secure client-proxy architecture — protecting API keys server-side while providing sub-second cached results.',
			metrics: [
				{ label: 'Cache Hit Speed', value: '<50ms' },
				{ label: 'Key Security', value: '100%' },
				{ label: 'Filtered Noise', value: 'Heuristic' },
				{ label: 'Storage Limit', value: '5MB' }
			],
			stack: ['JavaScript', 'Chrome Extension API', 'Web Crypto', 'Vercel', 'OpenAI', 'Node.js'],
			live: '#',
			github: '#',
			bg: 'radial-gradient(ellipse at 20% 30%, rgba(73,160,120,0.38) 0%, transparent 55%), radial-gradient(ellipse at 80% 70%, rgba(33,104,105,0.45) 0%, transparent 50%), linear-gradient(145deg, #0d1f18 0%, #0a1a14 50%, #061009 100%)',
			accentColor: '#49a078',
			cardBorder: 'rgba(73,160,120,0.25)',
			artifacts: [
				{
					id: 1,
					shape: 'cookie',
					size: 110,
					top: '12%',
					left: '8%',
					dur: 7,
					delay: 0,
					color: 'rgba(156,197,161,0.22)',
					rotate: 15
				},
				{
					id: 2,
					shape: 'ring',
					size: 200,
					top: '60%',
					left: '76%',
					dur: 9,
					delay: 0.6,
					color: 'rgba(73,160,120,0.18)'
				},
				{
					id: 3,
					shape: 'cookie',
					size: 70,
					top: '75%',
					left: '12%',
					dur: 6,
					delay: 1.2,
					color: 'rgba(220,225,222,0.14)',
					rotate: -20
				},
				{
					id: 4,
					shape: 'pill',
					size: 140,
					top: '18%',
					left: '74%',
					dur: 8,
					delay: 0.4,
					color: 'rgba(33,104,105,0.28)',
					blur: 20
				},
				{
					id: 5,
					shape: 'circle',
					size: 90,
					top: '45%',
					left: '88%',
					dur: 7,
					delay: 1,
					color: 'rgba(73,160,120,0.15)',
					blur: 30
				}
			]
		},
		{
			id: 2,
			index: '02',
			title: 'WhisperBox',
			tagline: 'End-to-End Encrypted Messaging',
			description:
				'A cryptography-first messaging application implementing Hybrid E2EE via RSA-OAEP + AES-256-GCM entirely in the browser. Private keys never touch the server — wrapped with PBKDF2-derived keys and stored encrypted in IndexedDB.',
			metrics: [
				{ label: 'Encryption', value: 'AES-256' },
				{ label: 'Key Exchange', value: 'RSA-OAEP' },
				{ label: 'Key Derivation', value: 'PBKDF2' },
				{ label: 'Server Data', value: '0 Plaintext' }
			],
			stack: ['React 19', 'Web Crypto API', 'WebSockets', 'IndexedDB', 'Vite', 'CSS'],
			live: '#',
			github: '#',
			bg: 'radial-gradient(ellipse at 75% 20%, rgba(33,104,105,0.4) 0%, transparent 50%), radial-gradient(ellipse at 15% 80%, rgba(31,36,33,0.9) 0%, transparent 60%), linear-gradient(160deg, #081418 0%, #0c1e22 45%, #040c0e 100%)',
			accentColor: '#216869',
			cardBorder: 'rgba(33,104,105,0.3)',
			artifacts: [
				{
					id: 6,
					shape: 'lock',
					size: 100,
					top: '10%',
					left: '78%',
					dur: 8,
					delay: 0,
					color: 'rgba(33,104,105,0.3)',
					rotate: 0
				},
				{
					id: 7,
					shape: 'ring',
					size: 260,
					top: '-5%',
					left: '-5%',
					dur: 10,
					delay: 0.3,
					color: 'rgba(33,104,105,0.14)'
				},
				{
					id: 8,
					shape: 'lock',
					size: 60,
					top: '68%',
					left: '82%',
					dur: 7,
					delay: 1,
					color: 'rgba(156,197,161,0.18)',
					rotate: -10
				},
				{
					id: 9,
					shape: 'square',
					size: 120,
					top: '72%',
					left: '5%',
					dur: 9,
					delay: 0.7,
					color: 'rgba(33,104,105,0.2)',
					rotate: 18,
					blur: 10
				},
				{
					id: 10,
					shape: 'pill',
					size: 100,
					top: '35%',
					left: '84%',
					dur: 6,
					delay: 1.5,
					color: 'rgba(73,160,120,0.12)',
					blur: 25
				}
			]
		},
		{
			id: 3,
			index: '03',
			title: 'Blip',
			tagline: 'Real-Time Analytics Dashboard',
			description:
				'Production-grade streaming analytics platform processing 100 events/sec with a 90% render-reduction via 100ms throttled batching. Canvas-rendered ECharts, Zod-validated streams, and Pinia state — built for scale.',
			metrics: [
				{ label: 'Render Reduction', value: '90%' },
				{ label: 'Chart Render', value: '5ms' },
				{ label: 'Stream Throughput', value: '100 eps' },
				{ label: 'Memory Bound', value: '~2MB' }
			],
			stack: ['Vue 3', 'TypeScript', 'Pinia', 'ECharts', 'Zod', 'TailwindCSS'],
			live: '#',
			github: '#',
			bg: 'radial-gradient(ellipse at 60% 10%, rgba(156,197,161,0.22) 0%, transparent 45%), radial-gradient(ellipse at 10% 85%, rgba(73,160,120,0.28) 0%, transparent 50%), linear-gradient(135deg, #0f1a10 0%, #131f14 50%, #080e09 100%)',
			accentColor: '#9cc5a1',
			cardBorder: 'rgba(156,197,161,0.2)',
			artifacts: [
				{
					id: 11,
					shape: 'blob',
					size: 180,
					top: '8%',
					left: '70%',
					dur: 9,
					delay: 0,
					color: 'rgba(156,197,161,0.16)',
					blur: 20
				},
				{
					id: 12,
					shape: 'ring',
					size: 150,
					top: '65%',
					left: '10%',
					dur: 8,
					delay: 0.5,
					color: 'rgba(73,160,120,0.2)'
				},
				{
					id: 13,
					shape: 'square',
					size: 80,
					top: '20%',
					left: '6%',
					dur: 7,
					delay: 1,
					color: 'rgba(156,197,161,0.14)',
					rotate: 30
				},
				{
					id: 14,
					shape: 'circle',
					size: 60,
					top: '78%',
					left: '78%',
					dur: 6,
					delay: 0.8,
					color: 'rgba(73,160,120,0.2)',
					blur: 15
				},
				{
					id: 15,
					shape: 'pill',
					size: 160,
					top: '50%',
					left: '80%',
					dur: 10,
					delay: 1.3,
					color: 'rgba(156,197,161,0.1)',
					blur: 30
				}
			]
		}
	];

	// ── State ──────────────────────────────────────────────────────────────
	let activeIndex = $state(0);
	const active = $derived(projects[activeIndex]);

	// ── Refs ───────────────────────────────────────────────────────────────
	let sectionEl: HTMLElement;
	let cardEl: HTMLElement;
	let artifactsEl: HTMLElement;
	let contentEl: HTMLElement;
	let bgEl: HTMLElement;
	let transitioning = false;

	// ── Navigation ─────────────────────────────────────────────────────────
	function goTo(index: number) {
		if (transitioning || index === activeIndex) return;
		transitioning = true;

		const tl = gsap.timeline({
			onComplete: async () => {
				activeIndex = index;
				transitioning = false;
				await tick();
				animateIn();
			}
		});

		// Exit: card slams down, artifacts scatter, bg fades
		tl.to(cardEl, {
			y: 60,
			opacity: 0,
			scale: 0.94,
			duration: 0.38,
			ease: 'power2.in'
		})
			.to(
				artifactsEl?.children ? Array.from(artifactsEl.children) : [],
				{
					opacity: 0,
					scale: 0.7,
					duration: 0.3,
					ease: 'power2.in',
					stagger: 0.05
				},
				'-=0.3'
			)
			.to(
				bgEl,
				{
					opacity: 0,
					duration: 0.25,
					ease: 'power1.in'
				},
				'-=0.25'
			);
	}

	function animateIn() {
		const artifacts = artifactsEl?.children ? Array.from(artifactsEl.children) : [];

		gsap.set(bgEl, { opacity: 0 });
		gsap.set(cardEl, { y: 70, opacity: 0, scale: 0.93 });
		gsap.set(artifacts, { opacity: 0, scale: 0.6, y: 30 });

		const tl = gsap.timeline();
		tl.to(bgEl, { opacity: 1, duration: 0.55, ease: 'power2.out' })
			.to(
				artifacts,
				{
					opacity: 1,
					scale: 1,
					y: 0,
					duration: 0.6,
					ease: 'power3.out',
					stagger: { each: 0.08, from: 'random' }
				},
				'-=0.3'
			)
			.to(
				cardEl,
				{
					y: 0,
					opacity: 1,
					scale: 1,
					duration: 0.72,
					ease: 'power3.out'
				},
				'-=0.4'
			);
	}

	function next() {
		goTo((activeIndex + 1) % projects.length);
	}
	function previous() {
		goTo((activeIndex - 1 + projects.length) % projects.length);
	}

	onMount(() => {
		animateIn();
	});
</script>

<!-- ── Template ──────────────────────────────────────────────────────────── -->
<section class="projects-section" bind:this={sectionEl}>
	<!-- Dynamic background -->
	<div class="scene-bg" bind:this={bgEl} style="background:{active.bg}"></div>
	<div class="grid-noise"></div>
	<div class="vignette"></div>

	<!-- Floating artifacts -->
	<div class="artifacts-layer" bind:this={artifactsEl}>
		{#each active.artifacts as a (a.id)}
			<div
				class="artifact artifact-{a.shape}"
				style="
					width:{a.size}px;
					height:{a.size + (a.shape === 'pill' ? 0 : 0)}px;
					top:{a.top};
					left:{a.left};
					background:{a.color};
					filter:blur({a.blur ?? 0}px);
					animation-duration:{a.dur}s;
					animation-delay:{a.delay}s;
					transform:rotate({a.rotate ?? 0}deg);
					border-color:{a.color};
				"
			>
				{#if a.imgSrc}
					<img src={a.imgSrc} alt="" />
				{/if}
			</div>
		{/each}
	</div>

	<!-- Main card -->
	<div class="card-wrap" bind:this={cardEl}>
		<div
			class="project-card"
			style="border-color:{active.cardBorder}; --accent:{active.accentColor}"
		>
			<!-- Card header bar -->
			<div class="card-bar">
				<span class="featured-label">Featured Project</span>
				<div class="paginator">
					<span class="pag-current" style="color:{active.accentColor}">{active.index}</span>
					<div class="pag-track">
						{#each projects as p, i}
							<button
								class="pag-dot"
								class:active={i === activeIndex}
								style={i === activeIndex ? `background:${active.accentColor}` : ''}
								on:click={() => goTo(i)}
								aria-label="Go to project {i + 1}"
							></button>
						{/each}
					</div>
					<span class="pag-total">{String(projects.length).padStart(2, '0')}</span>
				</div>
			</div>

			<!-- Card body -->
			<div class="card-body">
				<div class="card-left">
					<p class="project-tagline" style="color:{active.accentColor}">{active.tagline}</p>
					<h2 class="project-title">{active.title}</h2>
					<p class="project-desc">{active.description}</p>

					<!-- Metrics row -->
					<div class="metrics-row">
						{#each active.metrics as m}
							<div class="metric-block">
								<span class="metric-value" style="color:{active.accentColor}">{m.value}</span>
								<span class="metric-label">{m.label}</span>
							</div>
						{/each}
					</div>

					<!-- Stack pills -->
					<div class="stack-row">
						{#each active.stack as tech}
							<span class="stack-pill" style="border-color:{active.cardBorder}">{tech}</span>
						{/each}
					</div>
				</div>

				<!-- Right: links + image placeholder -->
				<div class="card-right">
					<div class="image-placeholder">
						<svg width="52" height="52" viewBox="0 0 52 52" fill="none">
							<rect
								x="4"
								y="4"
								width="44"
								height="44"
								rx="12"
								stroke="currentColor"
								stroke-width="1.5"
								stroke-dasharray="4 3"
							/>
							<circle cx="19" cy="20" r="5" stroke="currentColor" stroke-width="1.5" />
							<path
								d="M4 36l13-10 8 7 8-9 15 14"
								stroke="currentColor"
								stroke-width="1.5"
								stroke-linecap="round"
								stroke-linejoin="round"
							/>
						</svg>
						<span>Project Screenshot</span>
					</div>

					<div class="link-group">
						<a
							href={active.live}
							target="_blank"
							class="link-btn link-primary"
							style="background:{active.accentColor}20; border-color:{active.cardBorder}; --a:{active.accentColor}"
						>
							<svg width="14" height="14" viewBox="0 0 14 14" fill="none"
								><path
									d="M1 7h12M8 2l5 5-5 5"
									stroke="currentColor"
									stroke-width="1.5"
									stroke-linecap="round"
									stroke-linejoin="round"
								/></svg
							>
							Live Preview
						</a>
						<a
							href={active.github}
							target="_blank"
							class="link-btn"
							style="border-color:{active.cardBorder}"
						>
							<svg width="14" height="14" viewBox="0 0 14 14" fill="none"
								><path
									d="M7 1C3.686 1 1 3.686 1 7c0 2.656 1.727 4.904 4.124 5.698.3.055.41-.13.41-.29 0-.143-.006-.52-.008-1.022-1.671.363-2.024-.806-2.024-.806-.273-.694-.667-.88-.667-.88-.545-.373.041-.365.041-.365.603.042.92.619.92.619.536.918 1.406.653 1.748.499.054-.388.209-.653.38-.802-1.335-.152-2.738-.668-2.738-2.97 0-.656.234-1.193.619-1.613-.062-.152-.268-.763.058-1.59 0 0 .505-.162 1.655.617.48-.134 1.026-.2 1.553-.203.527.003 1.073.069 1.553.203 1.149-.779 1.653-.617 1.653-.617.327.827.121 1.438.059 1.59.386.42.619.957.619 1.613 0 2.308-1.405 2.816-2.744 2.965.217.187.409.555.409 1.115 0 .806-.007 1.455-.007 1.652 0 .161.109.349.414.29C11.275 11.902 13 9.655 13 7c0-3.314-2.686-6-6-6z"
									fill="currentColor"
								/></svg
							>
							GitHub
						</a>
					</div>
				</div>
			</div>
		</div>

		<!-- Navigation -->
		<div class="nav-controls">
			<button class="nav-btn" on:click={previous} style="--a:{active.accentColor}">
				<svg width="18" height="18" viewBox="0 0 18 18" fill="none"
					><path
						d="M11 4L6 9l5 5"
						stroke="currentColor"
						stroke-width="1.6"
						stroke-linecap="round"
						stroke-linejoin="round"
					/></svg
				>
				Prev
			</button>
			<div class="nav-label" style="color:{active.accentColor}">
				{active.index} / {String(projects.length).padStart(2, '0')}
			</div>
			<button class="nav-btn" on:click={next} style="--a:{active.accentColor}">
				Next
				<svg width="18" height="18" viewBox="0 0 18 18" fill="none"
					><path
						d="M7 4l5 5-5 5"
						stroke="currentColor"
						stroke-width="1.6"
						stroke-linecap="round"
						stroke-linejoin="round"
					/></svg
				>
			</button>
		</div>
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
	}

	/* ── Section shell ── */
	.projects-section {
		position: relative;
		min-height: 100svh;
		width: 100%;
		overflow: hidden;
		display: flex;
		align-items: center;
		justify-content: center;
		scroll-snap-align: start;
		scroll-snap-stop: always;
		font-family: 'DM Sans', 'Outfit', system-ui, sans-serif;
		isolation: isolate;
		padding: 3rem 1.5rem;
	}

	/* ── Dynamic background ── */
	.scene-bg {
		position: absolute;
		inset: 0;
		z-index: 0;
		transition: background 0.9s ease;
	}

	/* ── Grid noise overlay ── */
	.grid-noise {
		position: absolute;
		inset: 0;
		z-index: 1;
		pointer-events: none;
		opacity: 0.032;
		background-image:
			linear-gradient(rgba(255, 255, 255, 0.1) 1px, transparent 1px),
			linear-gradient(90deg, rgba(255, 255, 255, 0.07) 1px, transparent 1px);
		background-size: 3.5rem 3.5rem;
	}

	/* ── Vignette ── */
	.vignette {
		position: absolute;
		inset: 0;
		z-index: 2;
		pointer-events: none;
		background: radial-gradient(ellipse at 50% 50%, transparent 40%, rgba(0, 0, 0, 0.55) 100%);
	}

	/* ── Artifacts ── */
	.artifacts-layer {
		position: absolute;
		inset: 0;
		z-index: 3;
		pointer-events: none;
	}

	.artifact {
		position: absolute;
		animation: float ease-in-out infinite;
		border-radius: 999px;
	}

	.artifact-ring {
		background: transparent !important;
		border: 1.5px solid;
		border-radius: 999px;
	}
	.artifact-square {
		border-radius: 14px;
		animation: floatspin ease-in-out infinite;
	}
	.artifact-pill {
		border-radius: 999px;
		height: 44px !important;
	}
	.artifact-cookie {
		border-radius: 40% 60% 55% 45% / 45% 40% 60% 55%;
		animation: floatspin ease-in-out infinite;
	}
	.artifact-lock {
		border-radius: 14px 14px 10px 10px;
		display: flex;
		align-items: center;
		justify-content: center;
	}
	.artifact-blob {
		border-radius: 60% 40% 55% 45% / 50% 60% 40% 50%;
		animation: floatblob ease-in-out infinite;
	}
	.artifact img {
		width: 100%;
		height: 100%;
		object-fit: contain;
		opacity: 0.75;
	}

	@keyframes float {
		0%,
		100% {
			transform: translateY(0px) rotate(var(--r, 0deg));
		}
		50% {
			transform: translateY(-16px) rotate(calc(var(--r, 0deg) + 4deg));
		}
	}
	@keyframes floatspin {
		0%,
		100% {
			transform: translateY(0px) rotate(0deg);
		}
		50% {
			transform: translateY(-14px) rotate(12deg);
		}
	}
	@keyframes floatblob {
		0%,
		100% {
			transform: translateY(0px) scale(1) rotate(0deg);
		}
		33% {
			transform: translateY(-12px) scale(1.06) rotate(6deg);
		}
		66% {
			transform: translateY(-6px) scale(0.96) rotate(-4deg);
		}
	}

	/* ── Card wrap ── */
	.card-wrap {
		position: relative;
		z-index: 10;
		width: min(1140px, 100%);
		display: flex;
		flex-direction: column;
		align-items: center;
		gap: 1.75rem;
	}

	/* ── Project card ── */
	.project-card {
		width: 100%;
		border-radius: 28px;
		border: 1px solid;
		background: rgba(10, 20, 15, 0.72);
		backdrop-filter: blur(28px) saturate(1.3);
		-webkit-backdrop-filter: blur(28px) saturate(1.3);
		box-shadow:
			0 32px 80px rgba(0, 0, 0, 0.5),
			0 1px 0 rgba(255, 255, 255, 0.06) inset;
		overflow: hidden;
		transition: box-shadow 0.4s ease;
	}
	.project-card:hover {
		box-shadow:
			0 40px 100px rgba(0, 0, 0, 0.6),
			0 1px 0 rgba(255, 255, 255, 0.08) inset;
	}

	/* Card bar */
	.card-bar {
		display: flex;
		align-items: center;
		justify-content: space-between;
		padding: 1.1rem 2rem;
		border-bottom: 1px solid rgba(255, 255, 255, 0.06);
	}
	.featured-label {
		font-size: 0.62rem;
		font-weight: 700;
		letter-spacing: 0.26em;
		text-transform: uppercase;
		color: rgba(220, 225, 222, 0.45);
	}
	.paginator {
		display: flex;
		align-items: center;
		gap: 0.75rem;
	}
	.pag-current,
	.pag-total {
		font-size: 0.72rem;
		font-weight: 700;
		font-variant-numeric: tabular-nums;
		letter-spacing: 0.1em;
	}
	.pag-total {
		color: rgba(220, 225, 222, 0.3);
	}
	.pag-track {
		display: flex;
		align-items: center;
		gap: 0.4rem;
	}
	.pag-dot {
		width: 6px;
		height: 6px;
		border-radius: 50%;
		background: rgba(255, 255, 255, 0.18);
		border: none;
		cursor: pointer;
		padding: 0;
		transition:
			background 0.3s ease,
			transform 0.3s ease;
	}
	.pag-dot.active {
		transform: scale(1.4);
	}

	/* Card body */
	.card-body {
		display: grid;
		grid-template-columns: 1fr 320px;
		gap: 2.5rem;
		padding: 2.5rem 2rem;
		align-items: start;
	}

	.project-tagline {
		font-size: 0.65rem;
		font-weight: 700;
		letter-spacing: 0.24em;
		text-transform: uppercase;
		margin: 0 0 0.7rem;
	}
	.project-title {
		font-size: clamp(2.2rem, 4.5vw, 4rem);
		font-weight: 800;
		letter-spacing: -0.04em;
		line-height: 1;
		color: var(--c-mist);
		margin: 0 0 1.1rem;
	}
	.project-desc {
		font-size: 0.9rem;
		line-height: 1.75;
		color: rgba(220, 225, 222, 0.6);
		max-width: 52ch;
		margin: 0;
	}

	/* Metrics */
	.metrics-row {
		display: flex;
		gap: 1.5rem;
		margin-top: 1.75rem;
		flex-wrap: wrap;
	}
	.metric-block {
		display: flex;
		flex-direction: column;
		gap: 0.2rem;
	}
	.metric-value {
		font-size: 1.3rem;
		font-weight: 800;
		letter-spacing: -0.03em;
		line-height: 1;
		font-variant-numeric: tabular-nums;
	}
	.metric-label {
		font-size: 0.6rem;
		font-weight: 600;
		letter-spacing: 0.12em;
		text-transform: uppercase;
		color: rgba(220, 225, 222, 0.38);
	}

	/* Stack */
	.stack-row {
		display: flex;
		flex-wrap: wrap;
		gap: 0.5rem;
		margin-top: 1.5rem;
	}
	.stack-pill {
		font-size: 0.68rem;
		font-weight: 600;
		letter-spacing: 0.04em;
		padding: 0.35rem 0.8rem;
		border-radius: 999px;
		border: 1px solid;
		color: rgba(220, 225, 222, 0.7);
		background: rgba(255, 255, 255, 0.05);
	}

	/* Right column */
	.card-right {
		display: flex;
		flex-direction: column;
		gap: 1.25rem;
	}

	.image-placeholder {
		width: 100%;
		aspect-ratio: 16/10;
		border-radius: 16px;
		background: rgba(255, 255, 255, 0.04);
		border: 1px dashed rgba(255, 255, 255, 0.1);
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		gap: 0.6rem;
		color: rgba(220, 225, 222, 0.25);
		font-size: 0.65rem;
		font-weight: 500;
		letter-spacing: 0.08em;
	}

	.link-group {
		display: flex;
		flex-direction: column;
		gap: 0.65rem;
	}
	.link-btn {
		display: flex;
		align-items: center;
		justify-content: center;
		gap: 0.5rem;
		padding: 0.75rem 1rem;
		border-radius: 12px;
		border: 1px solid;
		background: rgba(255, 255, 255, 0.05);
		color: rgba(220, 225, 222, 0.8);
		text-decoration: none;
		font-size: 0.78rem;
		font-weight: 600;
		letter-spacing: 0.04em;
		transition:
			background 0.25s ease,
			color 0.25s ease,
			transform 0.3s cubic-bezier(0.34, 1.56, 0.64, 1);
	}
	.link-btn:hover {
		background: rgba(255, 255, 255, 0.1);
		color: #fff;
		transform: translateY(-2px);
	}
	.link-primary:hover {
		background: color-mix(in srgb, var(--a) 30%, transparent);
	}

	/* ── Navigation ── */
	.nav-controls {
		display: flex;
		align-items: center;
		gap: 1rem;
	}
	.nav-btn {
		display: inline-flex;
		align-items: center;
		gap: 0.45rem;
		padding: 0.7rem 1.4rem;
		border-radius: 999px;
		border: 1px solid rgba(255, 255, 255, 0.1);
		background: rgba(255, 255, 255, 0.06);
		color: rgba(220, 225, 222, 0.75);
		font-size: 0.78rem;
		font-weight: 600;
		cursor: pointer;
		backdrop-filter: blur(12px);
		transition:
			background 0.25s ease,
			border-color 0.25s ease,
			transform 0.3s cubic-bezier(0.34, 1.56, 0.64, 1),
			color 0.25s ease;
	}
	.nav-btn:hover {
		background: rgba(255, 255, 255, 0.12);
		border-color: var(--a, rgba(255, 255, 255, 0.2));
		color: #fff;
		transform: scale(1.04);
	}
	.nav-label {
		font-size: 0.68rem;
		font-weight: 700;
		letter-spacing: 0.16em;
		font-variant-numeric: tabular-nums;
		min-width: 4rem;
		text-align: center;
	}

	/* ── Responsive ── */
	@media (max-width: 900px) {
		.card-body {
			grid-template-columns: 1fr;
			padding: 2rem 1.5rem;
		}
		.card-right {
			flex-direction: row;
			flex-wrap: wrap;
		}
		.image-placeholder {
			display: none;
		}
		.link-group {
			flex-direction: row;
			flex: 1;
		}
		.link-btn {
			flex: 1;
		}
		.metrics-row {
			gap: 1rem;
		}
	}
	@media (max-width: 540px) {
		.card-bar {
			padding: 1rem 1.25rem;
		}
		.project-title {
			font-size: 2rem;
		}
		.nav-controls {
			width: 100%;
			justify-content: center;
		}
	}
</style>
