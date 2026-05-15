<script lang="ts">
	import { onMount } from 'svelte';
	import gsap from 'gsap';

	let sectionRef: HTMLElement;
	let container: HTMLDivElement;
	let headerRef: HTMLDivElement;

	onMount(() => {
		const cards = Array.from(container?.querySelectorAll<HTMLElement>('[data-card]') ?? []);
		if (!cards.length) return;

		gsap.set(headerRef, { y: 28, opacity: 0 });
		gsap.set(cards, { y: 70, opacity: 0, scale: 0.93 });

		let bounceMaster: gsap.core.Timeline | null = null;
		let isInView = false;

		const startBounce = () => {
			if (bounceMaster) return;
			bounceMaster = gsap.timeline({ repeat: -1 });
			cards.forEach((card, i) => {
				bounceMaster!
					.to(
						card,
						{
							y: -12,
							scale: 1.025,
							boxShadow: '0 24px 52px rgba(31,36,33,0.13), 0 0 0 1.5px rgba(73,160,120,0.3)',
							duration: 0.38,
							ease: 'power2.out'
						},
						i * 0.28
					)
					.to(
						card,
						{
							y: 0,
							scale: 1,
							boxShadow: '0 2px 0 rgba(255,255,255,0.95) inset, 0 8px 32px rgba(31,36,33,0.07)',
							duration: 0.52,
							ease: 'elastic.out(1, 0.55)'
						},
						i * 0.28 + 0.38
					);
			});
			bounceMaster.to({}, { duration: 0.7 });
		};

		const stopBounce = () => {
			if (!bounceMaster) return;
			bounceMaster.kill();
			bounceMaster = null;
			gsap.to(cards, { y: 0, scale: 1, duration: 0.4, ease: 'power2.out', overwrite: true });
		};

		const animateIn = () => {
			if (isInView) return;
			isInView = true;
			stopBounce();
			gsap.set(headerRef, { y: 28, opacity: 0 });
			gsap.set(cards, { y: 70, opacity: 0, scale: 0.93 });
			const tl = gsap.timeline({
				defaults: { ease: 'power3.out' },
				onComplete: () => {
					if (isInView) startBounce();
				}
			});
			tl.to(headerRef, { y: 0, opacity: 1, duration: 0.62 }).to(
				cards,
				{
					y: 0,
					opacity: 1,
					scale: 1,
					duration: 0.68,
					stagger: { each: 0.13, from: 'start' }
				},
				'-=0.3'
			);
		};

		const animateOut = () => {
			if (!isInView) return;
			isInView = false;
			stopBounce();
			gsap.to(headerRef, { y: -20, opacity: 0, duration: 0.4, ease: 'power2.in' });
			gsap.to(cards, {
				y: 50,
				opacity: 0,
				scale: 0.94,
				duration: 0.45,
				ease: 'power2.in',
				stagger: { each: 0.07, from: 'end' }
			});
		};

		const observer = new IntersectionObserver(
			(entries) => entries.forEach((e) => (e.isIntersecting ? animateIn() : animateOut())),
			{ threshold: 0.2 }
		);
		observer.observe(sectionRef);
		return () => {
			observer.disconnect();
			stopBounce();
		};
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

<section class="edu-section" bind:this={sectionRef}>
	<div class="blob blob-a"></div>
	<div class="blob blob-b"></div>
	<div class="blob blob-c"></div>
	<div class="grain"></div>

	<div class="header-wrap" bind:this={headerRef}>
		<span class="eyebrow">Selected Background</span>
		<h2 class="section-title">Education</h2>
		<div class="title-rule"></div>
	</div>

	<div class="card-grid" bind:this={container}>
		{#each education as item, i}
			<div class="card-col" class:offset={i % 2 !== 0}>
				<article data-card class="card">
					<div class="card-stripe"></div>
					<div class="card-inner">
						<div class="card-meta">
							<span class="card-index">{item.index}</span>
							<span class="card-year">{item.year}</span>
						</div>
						<h3 class="card-title">{item.title}</h3>
						<p class="card-school">{item.school}</p>
						<div class="card-rule"></div>
						<p class="card-desc">{item.description}</p>
						<div class="card-dot"></div>
					</div>
				</article>
			</div>
		{/each}
	</div>
</section>

<style>
	:root {
		--c-dark: #1f2421;
		--c-teal: #216869;
		--c-green: #49a078;
		--c-sage: #9cc5a1;
		--c-mist: #dce1de;
		--c-bg: #f4f6f4;
		--c-surface: rgba(255, 255, 255, 0.78);
		--c-border: rgba(73, 160, 120, 0.16);
		--c-shadow: rgba(31, 36, 33, 0.07);
	}

	.edu-section {
		position: relative;
		min-height: 100svh;
		overflow: hidden;
		background: var(--c-bg);
		padding: 7rem 1.5rem 8rem;
		font-family: 'DM Sans', 'Outfit', system-ui, sans-serif;
	}

	.blob {
		position: absolute;
		border-radius: 50%;
		filter: blur(90px);
		pointer-events: none;
		z-index: 0;
	}
	.blob-a {
		width: 560px;
		height: 480px;
		top: -80px;
		left: -140px;
		background: radial-gradient(ellipse, rgba(156, 197, 161, 0.3) 0%, transparent 70%);
	}
	.blob-b {
		width: 440px;
		height: 400px;
		bottom: -60px;
		right: -100px;
		background: radial-gradient(ellipse, rgba(73, 160, 120, 0.2) 0%, transparent 70%);
	}
	.blob-c {
		width: 380px;
		height: 360px;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		background: radial-gradient(ellipse, rgba(33, 104, 105, 0.08) 0%, transparent 70%);
	}

	.grain {
		position: absolute;
		inset: 0;
		z-index: 1;
		pointer-events: none;
		opacity: 0.02;
		background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
		background-repeat: repeat;
		background-size: 200px;
	}

	.header-wrap {
		position: relative;
		z-index: 2;
		text-align: center;
		margin-bottom: 5rem;
	}
	.eyebrow {
		display: inline-block;
		font-size: 0.62rem;
		font-weight: 700;
		letter-spacing: 0.3em;
		text-transform: uppercase;
		color: var(--c-teal);
		background: rgba(33, 104, 105, 0.08);
		border: 1px solid rgba(33, 104, 105, 0.18);
		padding: 0.32rem 0.85rem;
		border-radius: 999px;
		margin-bottom: 0.9rem;
	}
	.section-title {
		display: block;
		font-size: clamp(2.6rem, 5vw, 4rem);
		font-weight: 800;
		letter-spacing: -0.04em;
		color: var(--c-dark);
		line-height: 1;
		margin: 0.75rem 0 1.1rem;
	}
	.title-rule {
		width: 38px;
		height: 3px;
		background: linear-gradient(90deg, var(--c-green), var(--c-sage));
		border-radius: 2px;
		margin: 0 auto;
	}

	.card-grid {
		position: relative;
		z-index: 2;
		display: grid;
		grid-template-columns: 1fr;
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

	.card {
		position: relative;
		border-radius: 22px;
		background: var(--c-surface);
		border: 1px solid var(--c-border);
		box-shadow:
			0 2px 0 rgba(255, 255, 255, 0.95) inset,
			0 8px 32px var(--c-shadow);
		backdrop-filter: blur(20px) saturate(1.5);
		-webkit-backdrop-filter: blur(20px) saturate(1.5);
		overflow: hidden;
		cursor: default;
		transition:
			transform 0.4s cubic-bezier(0.34, 1.56, 0.64, 1),
			box-shadow 0.4s ease,
			background 0.35s ease;
	}
	.card:hover {
		transform: translateY(-8px) scale(1.015);
		background: rgba(255, 255, 255, 0.92);
		box-shadow:
			0 2px 0 rgba(255, 255, 255, 0.98) inset,
			0 20px 52px rgba(31, 36, 33, 0.11),
			0 0 0 1px rgba(73, 160, 120, 0.22);
	}

	.card-stripe {
		position: absolute;
		top: 1.1rem;
		bottom: 1.1rem;
		left: 0;
		width: 3px;
		background: linear-gradient(180deg, var(--c-green), var(--c-sage));
		border-radius: 0 2px 2px 0;
		opacity: 0;
		transform: scaleY(0.4);
		transform-origin: top;
		transition:
			opacity 0.35s ease,
			transform 0.4s ease;
	}
	.card:hover .card-stripe {
		opacity: 1;
		transform: scaleY(1);
	}

	.card-inner {
		padding: 1.5rem 1.5rem 1.5rem 1.75rem;
		position: relative;
	}
	.card-meta {
		display: flex;
		align-items: center;
		justify-content: space-between;
		margin-bottom: 1rem;
	}
	.card-index {
		font-size: 0.62rem;
		font-weight: 800;
		letter-spacing: 0.14em;
		color: var(--c-green);
		font-variant-numeric: tabular-nums;
	}
	.card-year {
		font-size: 0.58rem;
		font-weight: 500;
		letter-spacing: 0.14em;
		text-transform: uppercase;
		color: #8a9e94;
	}
	.card-title {
		font-size: 0.96rem;
		font-weight: 700;
		letter-spacing: -0.02em;
		line-height: 1.25;
		color: var(--c-dark);
		margin: 0 0 0.3rem;
	}
	.card-school {
		font-size: 0.7rem;
		font-weight: 600;
		color: var(--c-teal);
		margin: 0;
		letter-spacing: 0.02em;
	}
	.card-rule {
		width: 22px;
		height: 1.5px;
		background: linear-gradient(90deg, var(--c-sage), transparent);
		border-radius: 1px;
		margin: 0.9rem 0;
	}
	.card-desc {
		font-size: 0.76rem;
		line-height: 1.68;
		color: #4e635a;
		margin: 0;
	}
	.card-dot {
		position: absolute;
		bottom: 1.1rem;
		right: 1.1rem;
		width: 5px;
		height: 5px;
		border-radius: 50%;
		background: var(--c-mist);
		border: 1.5px solid var(--c-sage);
		opacity: 0.6;
		transition:
			opacity 0.3s ease,
			background 0.3s ease;
	}
	.card:hover .card-dot {
		opacity: 1;
		background: var(--c-sage);
	}
</style>
