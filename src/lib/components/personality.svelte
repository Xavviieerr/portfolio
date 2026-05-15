<script lang="ts">
	import { onMount } from 'svelte';
	import gsap from 'gsap';

	let sectionRef: HTMLElement;
	let photoCard: HTMLElement;
	let leftRef: HTMLElement;
	let rightLines: HTMLElement;
	let bounceTl: gsap.core.Timeline | null = null;
	let isInView = false;

	const manifesto = [
		{ text: 'I build things', accent: false },
		{ text: 'that feel', accent: false },
		{ text: 'alive.', accent: true }
	];

	const beliefs = [
		'Craft is care made visible.',
		'Speed is a design decision.',
		'Motion is meaning.',
		'Every pixel is a promise.'
	];

	onMount(() => {
		// ── Set initial hidden states ──
		gsap.set(leftRef, { x: -60, opacity: 0 });
		gsap.set(photoCard, { scale: 0.85, opacity: 0, rotate: -4 });
		gsap.set(rightLines, { x: 40, opacity: 0 });

		const startBounce = () => {
			if (bounceTl) return;
			bounceTl = gsap.timeline({ repeat: -1, yoyo: false });
			bounceTl
				.to(photoCard, {
					y: -22,
					rotate: 2,
					scale: 1.025,
					boxShadow: '0 60px 100px rgba(31,36,33,0.22), 0 0 0 2px rgba(73,160,120,0.35)',
					duration: 1.6,
					ease: 'sine.inOut'
				})
				.to(photoCard, {
					y: 0,
					rotate: -1,
					scale: 1,
					boxShadow: '0 32px 64px rgba(31,36,33,0.14), 0 0 0 1px rgba(73,160,120,0.18)',
					duration: 1.6,
					ease: 'sine.inOut'
				});
		};

		const stopBounce = () => {
			if (!bounceTl) return;
			bounceTl.kill();
			bounceTl = null;
			gsap.to(photoCard, {
				y: 0,
				rotate: -1,
				scale: 1,
				duration: 0.5,
				ease: 'power2.out',
				overwrite: true
			});
		};

		const animateIn = () => {
			if (isInView) return;
			isInView = true;
			stopBounce();

			gsap.set(leftRef, { x: -60, opacity: 0 });
			gsap.set(photoCard, { scale: 0.85, opacity: 0, rotate: -4 });
			gsap.set(rightLines, { x: 40, opacity: 0 });

			const tl = gsap.timeline({
				defaults: { ease: 'power3.out' },
				onComplete: () => {
					if (isInView) startBounce();
				}
			});

			tl.to(leftRef, { x: 0, opacity: 1, duration: 0.85 })
				.to(
					photoCard,
					{ scale: 1, opacity: 1, rotate: -1, duration: 1, ease: 'elastic.out(1, 0.62)' },
					'-=0.5'
				)
				.to(rightLines, { x: 0, opacity: 1, duration: 0.75 }, '-=0.6');
		};

		const animateOut = () => {
			if (!isInView) return;
			isInView = false;
			stopBounce();
			gsap.to(leftRef, { x: -40, opacity: 0, duration: 0.45, ease: 'power2.in' });
			gsap.to(photoCard, { scale: 0.9, opacity: 0, rotate: 3, duration: 0.45, ease: 'power2.in' });
			gsap.to(rightLines, { x: 30, opacity: 0, duration: 0.45, ease: 'power2.in' });
		};

		const observer = new IntersectionObserver(
			(entries) => entries.forEach((e) => (e.isIntersecting ? animateIn() : animateOut())),
			{ threshold: 0.25 }
		);
		observer.observe(sectionRef);
		return () => {
			observer.disconnect();
			stopBounce();
		};
	});
</script>

<section class="split-section" bind:this={sectionRef}>
	<!-- ── LEFT: Light world ── -->
	<div class="left-world">
		<div class="left-grain"></div>
		<div class="left-blob blob-1"></div>
		<div class="left-blob blob-2"></div>

		<div class="left-content" bind:this={leftRef}>
			<span class="left-eyebrow">
				<span class="eyebrow-dot"></span>
				The Person
			</span>

			<div class="left-name-block">
				<h2 class="left-name">Chidera<br /><em>Ogbu</em></h2>
				<div class="name-rule"></div>
			</div>

			<div class="left-tags">
				<span class="tag">Frontend Engineer</span>
				<span class="tag-sep">·</span>
				<span class="tag">Interaction Designer</span>
			</div>

			<p class="left-bio">
				I sit at the intersection of engineering precision and design sensibility — building systems
				that respect users' time, attention, and intelligence.
			</p>

			<!-- Stat trio -->
			<div class="stat-trio">
				<div class="stat">
					<span class="stat-val">3+</span>
					<span class="stat-label">Years building</span>
				</div>
				<div class="stat-divider"></div>
				<div class="stat">
					<span class="stat-val">12+</span>
					<span class="stat-label">Projects shipped</span>
				</div>
				<div class="stat-divider"></div>
				<div class="stat">
					<span class="stat-val">∞</span>
					<span class="stat-label">Curiosity</span>
				</div>
			</div>
		</div>
	</div>

	<!-- ── DIVIDER: Organic shape ── -->
	<div class="organic-shape"></div>

	<!-- ── RIGHT: Dark world ── -->
	<div class="right-world">
		<div class="right-grain"></div>
		<div class="right-glow"></div>

		<div class="right-content" bind:this={rightLines}>
			<!-- Manifesto lines -->
			<div class="manifesto">
				{#each manifesto as line}
					<span class="manifesto-line" class:accent={line.accent}>{line.text}</span>
				{/each}
			</div>

			<!-- Beliefs list -->
			<ul class="beliefs">
				{#each beliefs as b, i}
					<li class="belief-item">
						<span class="belief-num">{String(i + 1).padStart(2, '0')}</span>
						<span class="belief-text">{b}</span>
					</li>
				{/each}
			</ul>

			<!-- Bottom line -->
			<p class="right-footer-text">Purpose isn't found — it's built, shipped, and iterated.</p>
		</div>
	</div>

	<!-- ── FLOATING PHOTO CARD (straddles the divide) ── -->
	<div class="photo-card" bind:this={photoCard}>
		<div class="photo-frame">
			<!-- Swap src with your actual photo -->
			<div class="photo-placeholder">
				<svg
					width="52"
					height="52"
					viewBox="0 0 52 52"
					fill="none"
					xmlns="http://www.w3.org/2000/svg"
				>
					<circle cx="26" cy="20" r="10" stroke="#9CC5A1" stroke-width="1.8" />
					<path
						d="M6 46c0-11.046 8.954-20 20-20s20 8.954 20 20"
						stroke="#9CC5A1"
						stroke-width="1.8"
						stroke-linecap="round"
					/>
				</svg>
				<span>Your Photo</span>
			</div>
		</div>

		<!-- Card label badge -->
		<div class="photo-badge">
			<span class="badge-dot"></span>
			Available
		</div>

		<!-- Decorative ring -->
		<div class="photo-ring"></div>
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
	}

	/* ── Section shell ── */
	.split-section {
		position: relative;
		min-height: 100svh;
		width: 100%;
		display: grid;
		grid-template-columns: 52% 48%;
		overflow: hidden;
		font-family: 'DM Sans', 'Outfit', system-ui, sans-serif;
	}

	/* ═══════════════════════════════
	   LEFT — light world
	═══════════════════════════════ */
	.left-world {
		position: relative;
		background: var(--c-bg);
		display: flex;
		align-items: center;
		padding: 6rem 3rem 6rem 5rem;
		z-index: 1;
		overflow: hidden;
	}

	.left-grain {
		position: absolute;
		inset: 0;
		pointer-events: none;
		opacity: 0.018;
		background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
		background-repeat: repeat;
		background-size: 200px;
	}

	.left-blob {
		position: absolute;
		border-radius: 50%;
		filter: blur(80px);
		pointer-events: none;
	}
	.blob-1 {
		width: 420px;
		height: 380px;
		top: -100px;
		left: -80px;
		background: radial-gradient(ellipse, rgba(156, 197, 161, 0.28) 0%, transparent 70%);
	}
	.blob-2 {
		width: 300px;
		height: 300px;
		bottom: -60px;
		right: 0;
		background: radial-gradient(ellipse, rgba(73, 160, 120, 0.15) 0%, transparent 70%);
	}

	.left-content {
		position: relative;
		z-index: 2;
		max-width: 420px;
	}

	.left-eyebrow {
		display: inline-flex;
		align-items: center;
		gap: 0.55rem;
		font-size: 0.62rem;
		font-weight: 700;
		letter-spacing: 0.28em;
		text-transform: uppercase;
		color: var(--c-teal);
		margin-bottom: 2rem;
	}
	.eyebrow-dot {
		width: 6px;
		height: 6px;
		border-radius: 50%;
		background: var(--c-green);
		box-shadow: 0 0 0 3px rgba(73, 160, 120, 0.2);
		animation: glow-pulse 2.4s ease-in-out infinite;
	}
	@keyframes glow-pulse {
		0%,
		100% {
			box-shadow: 0 0 0 3px rgba(73, 160, 120, 0.2);
		}
		50% {
			box-shadow: 0 0 0 7px rgba(73, 160, 120, 0.07);
		}
	}

	.left-name-block {
		margin-bottom: 1.25rem;
	}
	.left-name {
		font-size: clamp(3rem, 5.5vw, 5rem);
		font-weight: 900;
		letter-spacing: -0.05em;
		line-height: 0.95;
		color: var(--c-dark);
		margin: 0 0 0.9rem;
	}
	.left-name em {
		font-style: normal;
		color: var(--c-green);
		position: relative;
		display: inline-block;
	}
	.left-name em::after {
		content: '';
		position: absolute;
		bottom: 4px;
		left: 0;
		right: 0;
		height: 3px;
		background: linear-gradient(90deg, var(--c-green), var(--c-sage));
		border-radius: 2px;
		opacity: 0.4;
	}
	.name-rule {
		width: 44px;
		height: 2px;
		background: linear-gradient(90deg, var(--c-teal), transparent);
		border-radius: 2px;
	}

	.left-tags {
		display: flex;
		align-items: center;
		gap: 0.5rem;
		margin-bottom: 1.5rem;
		flex-wrap: wrap;
	}
	.tag {
		font-size: 0.7rem;
		font-weight: 600;
		color: #4d6b5e;
		letter-spacing: 0.02em;
	}
	.tag-sep {
		color: var(--c-sage);
	}

	.left-bio {
		font-size: 0.88rem;
		line-height: 1.78;
		color: #3d5247;
		margin: 0 0 2.25rem;
		max-width: 38ch;
	}

	/* Stat trio */
	.stat-trio {
		display: flex;
		align-items: center;
		gap: 1.25rem;
	}
	.stat {
		display: flex;
		flex-direction: column;
		gap: 0.15rem;
	}
	.stat-val {
		font-size: 1.6rem;
		font-weight: 900;
		letter-spacing: -0.04em;
		color: var(--c-dark);
		line-height: 1;
	}
	.stat-label {
		font-size: 0.58rem;
		font-weight: 600;
		letter-spacing: 0.1em;
		text-transform: uppercase;
		color: #8aaa96;
	}
	.stat-divider {
		width: 1px;
		height: 36px;
		background: linear-gradient(180deg, transparent, rgba(73, 160, 120, 0.3), transparent);
	}

	/* ═══════════════════════════════
	   ORGANIC DIVIDER SHAPE
	═══════════════════════════════ */
	.organic-shape {
		position: absolute;
		inset: 0;
		z-index: 2;
		pointer-events: none;
		/* A flowing S-curve that cuts across ~52% of the width */
		background: var(--c-dark);
		clip-path: polygon(58% 0%, 100% 0%, 100% 100%, 56% 100%, 50% 82%, 47% 60%, 49% 38%, 54% 18%);
	}

	/* ═══════════════════════════════
	   RIGHT — dark world
	═══════════════════════════════ */
	.right-world {
		position: relative;
		background: var(--c-dark);
		display: flex;
		align-items: center;
		padding: 6rem 5rem 6rem 7rem;
		z-index: 1;
		overflow: hidden;
	}

	.right-grain {
		position: absolute;
		inset: 0;
		pointer-events: none;
		opacity: 0.04;
		background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
		background-repeat: repeat;
		background-size: 200px;
	}
	.right-glow {
		position: absolute;
		width: 400px;
		height: 400px;
		top: -100px;
		right: -60px;
		border-radius: 50%;
		filter: blur(100px);
		background: radial-gradient(ellipse, rgba(33, 104, 105, 0.35) 0%, transparent 70%);
		pointer-events: none;
	}

	.right-content {
		position: relative;
		z-index: 2;
		max-width: 380px;
	}

	/* Manifesto */
	.manifesto {
		display: flex;
		flex-direction: column;
		margin-bottom: 2.5rem;
		line-height: 1;
	}
	.manifesto-line {
		font-size: clamp(2.4rem, 4.5vw, 3.6rem);
		font-weight: 900;
		letter-spacing: -0.05em;
		color: rgba(220, 225, 222, 0.18);
		line-height: 1.1;
		transition: color 0.3s;
	}
	.manifesto-line.accent {
		color: var(--c-sage);
		position: relative;
	}
	.manifesto-line.accent::after {
		content: '';
		position: absolute;
		bottom: 2px;
		left: 0;
		width: 100%;
		height: 3px;
		background: linear-gradient(90deg, var(--c-green), transparent);
		border-radius: 2px;
		opacity: 0.5;
	}

	/* Beliefs list */
	.beliefs {
		list-style: none;
		padding: 0;
		margin: 0 0 2.5rem;
		display: flex;
		flex-direction: column;
		gap: 0.85rem;
		border-left: 1px solid rgba(156, 197, 161, 0.15);
		padding-left: 1.25rem;
	}
	.belief-item {
		display: flex;
		align-items: baseline;
		gap: 0.7rem;
	}
	.belief-num {
		font-size: 0.55rem;
		font-weight: 800;
		letter-spacing: 0.14em;
		color: var(--c-green);
		font-variant-numeric: tabular-nums;
		flex-shrink: 0;
	}
	.belief-text {
		font-size: 0.85rem;
		font-weight: 500;
		color: rgba(220, 225, 222, 0.6);
		line-height: 1.5;
		letter-spacing: 0.01em;
	}

	.right-footer-text {
		font-size: 0.72rem;
		font-style: italic;
		color: rgba(220, 225, 222, 0.28);
		margin: 0;
		padding-top: 1.5rem;
		border-top: 1px solid rgba(220, 225, 222, 0.07);
		letter-spacing: 0.02em;
	}

	/* ═══════════════════════════════
	   FLOATING PHOTO CARD
	═══════════════════════════════ */
	.photo-card {
		position: absolute;
		left: 50%;
		top: 50%;
		transform: translate(-50%, -50%) rotate(-1deg);
		z-index: 10;
		width: 280px;
		height: 340px;
		border-radius: 28px;
		background: rgba(255, 255, 255, 0.92);
		backdrop-filter: blur(24px) saturate(1.6);
		-webkit-backdrop-filter: blur(24px) saturate(1.6);
		border: 1px solid rgba(255, 255, 255, 0.7);
		box-shadow:
			0 32px 64px rgba(31, 36, 33, 0.14),
			0 0 0 1px rgba(73, 160, 120, 0.18),
			0 2px 0 rgba(255, 255, 255, 0.95) inset;
		overflow: visible;
	}

	/* Decorative ring behind card */
	.photo-ring {
		position: absolute;
		inset: -16px;
		border-radius: 40px;
		border: 1.5px solid rgba(73, 160, 120, 0.14);
		pointer-events: none;
		z-index: -1;
	}

	.photo-frame {
		width: 100%;
		height: 100%;
		border-radius: 28px;
		overflow: hidden;
		background: linear-gradient(145deg, var(--c-mist) 0%, rgba(156, 197, 161, 0.4) 100%);
	}

	.photo-placeholder {
		width: 100%;
		height: 100%;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		gap: 0.75rem;
		color: var(--c-teal);
		font-size: 0.7rem;
		font-weight: 600;
		letter-spacing: 0.1em;
		opacity: 0.6;
	}

	/* Availability badge */
	.photo-badge {
		position: absolute;
		bottom: -14px;
		left: 50%;
		transform: translateX(-50%);
		display: inline-flex;
		align-items: center;
		gap: 0.45rem;
		background: var(--c-dark);
		border: 1px solid rgba(73, 160, 120, 0.3);
		padding: 0.4rem 0.9rem;
		border-radius: 999px;
		font-size: 0.65rem;
		font-weight: 700;
		letter-spacing: 0.1em;
		color: var(--c-sage);
		white-space: nowrap;
		box-shadow: 0 8px 20px rgba(0, 0, 0, 0.3);
	}
	.badge-dot {
		width: 6px;
		height: 6px;
		border-radius: 50%;
		background: var(--c-green);
		animation: glow-pulse 2.4s ease-in-out infinite;
	}

	/* ── Responsive ── */
	@media (max-width: 900px) {
		.split-section {
			grid-template-columns: 1fr;
			grid-template-rows: auto auto;
		}
		.left-world {
			padding: 5rem 2rem 8rem;
		}
		.right-world {
			padding: 8rem 2rem 5rem;
		}
		.organic-shape {
			clip-path: polygon(0% 45%, 100% 40%, 100% 55%, 0% 60%);
		}
		.photo-card {
			left: 50%;
			top: auto;
			bottom: -40px;
			transform: translateX(-50%) rotate(-1deg);
			width: 200px;
			height: 240px;
		}
		.right-content {
			margin-top: 3rem;
		}
		.manifesto-line {
			font-size: 2.2rem;
		}
	}
</style>
