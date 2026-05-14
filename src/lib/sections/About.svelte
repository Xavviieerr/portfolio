<script lang="ts">
	import { onMount } from 'svelte';
	import gsap from 'gsap';

	let imageCard: HTMLDivElement;
	let infoCard: HTMLDivElement;
	let sectionRef: HTMLElement;

	onMount(() => {
		// Lock both cards to their off-screen start positions immediately
		gsap.set(imageCard, { x: -72, opacity: 0, scale: 0.96 });
		gsap.set(infoCard, { x: 72, opacity: 0, scale: 0.96 });

		let enterTl: gsap.core.Timeline | null = null;
		let leaveTl: gsap.core.Timeline | null = null;

		const animateIn = () => {
			leaveTl?.kill();
			enterTl = gsap.timeline({ defaults: { ease: 'power3.out' } });
			enterTl
				.to(imageCard, { x: 0, opacity: 1, scale: 1, duration: 0.82 })
				.to(infoCard, { x: 0, opacity: 1, scale: 1, duration: 0.82 }, '-=0.58');
		};

		const animateOut = () => {
			enterTl?.kill();
			leaveTl = gsap.timeline({ defaults: { ease: 'power3.in' } });
			leaveTl
				.to(imageCard, { x: -72, opacity: 0, scale: 0.96, duration: 0.55 })
				.to(infoCard, { x: 72, opacity: 0, scale: 0.96, duration: 0.55 }, '-=0.45');
		};

		const observer = new IntersectionObserver(
			(entries) => {
				entries.forEach((entry) => {
					if (entry.isIntersecting) {
						animateIn();
					} else {
						animateOut();
					}
				});
			},
			{
				// Fire when at least 20% of the section is visible
				threshold: 0.2
			}
		);

		observer.observe(sectionRef);

		return () => observer.disconnect();
	});
</script>

<section class="about-section" bind:this={sectionRef}>
	<!-- Ambient blobs -->
	<div class="blob blob-a"></div>
	<div class="blob blob-b"></div>
	<div class="blob blob-c"></div>
	<div class="grain"></div>

	<div class="content-wrap">
		<div class="about-grid">
			<!-- ── IMAGE CARD ── -->
			<div class="glass-card image-card" bind:this={imageCard}>
				<div class="image-frame">
					<!-- Replace with your <img> -->
					<div class="image-placeholder">
						<svg
							width="48"
							height="48"
							viewBox="0 0 48 48"
							fill="none"
							xmlns="http://www.w3.org/2000/svg"
						>
							<circle cx="24" cy="18" r="9" stroke="#9CC5A1" stroke-width="2" />
							<path
								d="M6 42c0-9.941 8.059-18 18-18s18 8.059 18 18"
								stroke="#9CC5A1"
								stroke-width="2"
								stroke-linecap="round"
							/>
						</svg>
						<span>Your Photo Here</span>
					</div>
				</div>

				<!-- Floating stat chips -->
				<div class="stat-chip chip-top">
					<span class="chip-dot"></span>
					Available for work
				</div>
				<div class="stat-chip chip-bottom">
					<span class="chip-num">3+</span>
					<span class="chip-label">Years building</span>
				</div>
			</div>

			<!-- ── INFO CARD ── -->
			<div class="glass-card info-card" bind:this={infoCard}>
				<!-- Eyebrow -->
				<div class="eyebrow-row">
					<span class="eyebrow-pill">About Me</span>
				</div>

				<!-- Name -->
				<h2 class="name-heading">Your<br /><em>Name</em></h2>

				<!-- Role tags -->
				<div class="role-tags">
					<span class="tag">Frontend Engineer</span>
					<span class="tag-sep">·</span>
					<span class="tag">Interaction Design</span>
				</div>

				<!-- Divider -->
				<div class="info-rule"></div>

				<!-- Bio -->
				<p class="bio-text">
					I build immersive digital experiences that combine performance, motion, and intentional
					interaction design — creating interfaces that feel genuinely alive.
				</p>

				<!-- CTA row -->
				<div class="cta-row">
					<button class="btn-primary">
						<svg
							width="16"
							height="16"
							viewBox="0 0 16 16"
							fill="none"
							xmlns="http://www.w3.org/2000/svg"
						>
							<path
								d="M8 1v9m0 0L5 7m3 3l3-3M2 13h12"
								stroke="currentColor"
								stroke-width="1.6"
								stroke-linecap="round"
								stroke-linejoin="round"
							/>
						</svg>
						Download CV
					</button>

					<a href="#" class="link-pill">
						<svg
							width="15"
							height="15"
							viewBox="0 0 15 15"
							fill="none"
							xmlns="http://www.w3.org/2000/svg"
						>
							<path
								d="M7.5 1C3.91 1 1 3.91 1 7.5c0 2.87 1.863 5.306 4.447 6.168.325.06.443-.141.443-.313 0-.154-.006-.563-.009-1.106-1.807.393-2.188-.872-2.188-.872-.295-.75-.721-.95-.721-.95-.589-.403.045-.395.045-.395.651.046.994.669.994.669.579.992 1.518.705 1.888.539.059-.419.226-.705.411-.867-1.443-.164-2.96-.722-2.96-3.21 0-.709.253-1.288.669-1.742-.067-.165-.29-.824.063-1.717 0 0 .546-.175 1.787.666A6.22 6.22 0 017.5 4.98c.552.003 1.108.075 1.628.22 1.24-.841 1.785-.666 1.785-.666.354.893.131 1.552.064 1.717.417.454.668 1.033.668 1.742 0 2.495-1.52 3.044-2.967 3.205.234.201.442.598.442 1.205 0 .87-.008 1.571-.008 1.784 0 .174.117.376.447.312C12.14 12.803 14 10.368 14 7.5 14 3.91 11.09 1 7.5 1z"
								fill="currentColor"
							/>
						</svg>
						GitHub
					</a>

					<a href="#" class="link-pill">
						<svg
							width="15"
							height="15"
							viewBox="0 0 15 15"
							fill="none"
							xmlns="http://www.w3.org/2000/svg"
						>
							<path
								d="M2 1h11a1 1 0 011 1v11a1 1 0 01-1 1H2a1 1 0 01-1-1V2a1 1 0 011-1zm2.5 3a1 1 0 100 2 1 1 0 000-2zM3 6.5h3V12H3V6.5zm4 0h3v.75c.4-.55 1-.75 1.75-.75C13 6.5 13 8 13 9v3h-3V9.25c0-.45-.2-.75-.65-.75s-.85.3-.85.75V12H6V6.5z"
								fill="currentColor"
							/>
						</svg>
						LinkedIn
					</a>
				</div>

				<!-- Decorative index -->
				<span class="deco-index">©2025</span>
			</div>
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
		--c-bg: #f4f6f4;
		--c-surface: rgba(255, 255, 255, 0.68);
		--c-border: rgba(73, 160, 120, 0.16);
		--c-shadow: rgba(31, 36, 33, 0.09);
	}

	/* ── Section ── */
	.about-section {
		position: relative;
		min-height: 100svh;
		width: 100%;
		overflow: hidden;
		background: var(--c-bg);
		font-family: 'DM Sans', 'Outfit', system-ui, sans-serif;
	}

	/* ── Blobs ── */
	.blob {
		position: absolute;
		border-radius: 50%;
		filter: blur(90px);
		pointer-events: none;
		z-index: 0;
	}
	.blob-a {
		width: 600px;
		height: 500px;
		top: -140px;
		left: -160px;
		background: radial-gradient(ellipse, rgba(156, 197, 161, 0.26) 0%, transparent 70%);
	}
	.blob-b {
		width: 500px;
		height: 450px;
		top: 20%;
		right: -140px;
		background: radial-gradient(ellipse, rgba(33, 104, 105, 0.16) 0%, transparent 70%);
	}
	.blob-c {
		width: 420px;
		height: 380px;
		bottom: -60px;
		left: 35%;
		background: radial-gradient(ellipse, rgba(73, 160, 120, 0.14) 0%, transparent 70%);
	}

	/* ── Grain ── */
	.grain {
		position: absolute;
		inset: 0;
		z-index: 1;
		pointer-events: none;
		opacity: 0.022;
		background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
		background-repeat: repeat;
		background-size: 200px;
	}

	/* ── Content wrap ── */
	.content-wrap {
		position: relative;
		z-index: 2;
		max-width: 1280px;
		margin: 0 auto;
		min-height: 100svh;
		display: flex;
		align-items: center;
		padding: 6rem 1.5rem;
	}

	/* ── Grid ── */
	.about-grid {
		display: grid;
		grid-template-columns: 1fr;
		gap: 1.25rem;
		width: 100%;
	}
	@media (min-width: 768px) {
		.about-grid {
			grid-template-columns: 1fr 1fr;
			gap: 1.5rem;
		}
	}

	/* ── Shared glass card ── */
	.glass-card {
		border-radius: 28px;
		background: var(--c-surface);
		border: 1px solid var(--c-border);
		box-shadow:
			0 2px 0 rgba(255, 255, 255, 0.88) inset,
			0 16px 48px var(--c-shadow);
		backdrop-filter: blur(24px) saturate(1.5);
		-webkit-backdrop-filter: blur(24px) saturate(1.5);
		transition:
			box-shadow 0.4s ease,
			transform 0.4s cubic-bezier(0.34, 1.56, 0.64, 1);
	}
	.glass-card:hover {
		box-shadow:
			0 2px 0 rgba(255, 255, 255, 0.95) inset,
			0 28px 64px rgba(31, 36, 33, 0.13),
			0 0 0 1px rgba(73, 160, 120, 0.22);
	}

	/* ── Image card ── */
	.image-card {
		position: relative;
		padding: 1.25rem;
	}
	.image-card:hover {
		transform: translateY(-6px);
	}

	.image-frame {
		width: 100%;
		height: 520px;
		border-radius: 20px;
		overflow: hidden;
		background: linear-gradient(145deg, var(--c-mist) 0%, rgba(156, 197, 161, 0.35) 100%);
	}

	.image-placeholder {
		width: 100%;
		height: 100%;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		gap: 0.75rem;
		color: var(--c-teal);
		font-size: 0.78rem;
		font-weight: 500;
		letter-spacing: 0.08em;
		opacity: 0.7;
	}

	/* Floating chips */
	.stat-chip {
		position: absolute;
		display: flex;
		align-items: center;
		gap: 0.5rem;
		background: rgba(255, 255, 255, 0.88);
		border: 1px solid var(--c-border);
		backdrop-filter: blur(12px);
		border-radius: 100px;
		padding: 0.45rem 1rem;
		font-size: 0.7rem;
		font-weight: 600;
		color: var(--c-dark);
		box-shadow: 0 4px 16px rgba(31, 36, 33, 0.08);
		letter-spacing: 0.01em;
		white-space: nowrap;
	}
	.chip-top {
		top: 2rem;
		right: -1rem;
	}
	.chip-bottom {
		bottom: 2.2rem;
		left: -0.75rem;
		gap: 0.35rem;
	}

	.chip-dot {
		width: 7px;
		height: 7px;
		border-radius: 50%;
		background: var(--c-green);
		box-shadow: 0 0 0 3px rgba(73, 160, 120, 0.2);
		animation: pulse 2.4s ease-in-out infinite;
	}
	@keyframes pulse {
		0%,
		100% {
			box-shadow: 0 0 0 3px rgba(73, 160, 120, 0.2);
		}
		50% {
			box-shadow: 0 0 0 6px rgba(73, 160, 120, 0.08);
		}
	}

	.chip-num {
		font-size: 0.9rem;
		font-weight: 800;
		color: var(--c-green);
		line-height: 1;
	}
	.chip-label {
		font-size: 0.65rem;
		color: #5a6b61;
		font-weight: 500;
	}

	/* ── Info card ── */
	.info-card {
		position: relative;
		padding: 2.75rem 2.5rem;
		display: flex;
		flex-direction: column;
	}

	.eyebrow-row {
		margin-bottom: 1.5rem;
	}
	.eyebrow-pill {
		display: inline-block;
		font-size: 0.62rem;
		font-weight: 700;
		letter-spacing: 0.26em;
		text-transform: uppercase;
		color: var(--c-teal);
		background: rgba(33, 104, 105, 0.08);
		border: 1px solid rgba(33, 104, 105, 0.2);
		padding: 0.35rem 0.85rem;
		border-radius: 100px;
	}

	.name-heading {
		font-size: clamp(2.8rem, 5.5vw, 4.2rem);
		font-weight: 800;
		letter-spacing: -0.04em;
		line-height: 1.05;
		color: var(--c-dark);
		margin: 0 0 1rem;
	}
	.name-heading em {
		font-style: normal;
		color: var(--c-green);
		position: relative;
	}
	/* Underline accent on the italic name */
	.name-heading em::after {
		content: '';
		position: absolute;
		bottom: 4px;
		left: 0;
		right: 0;
		height: 3px;
		background: linear-gradient(90deg, var(--c-green), var(--c-sage));
		border-radius: 2px;
		opacity: 0.5;
	}

	.role-tags {
		display: flex;
		align-items: center;
		gap: 0.5rem;
		margin-bottom: 1.75rem;
		flex-wrap: wrap;
	}
	.tag {
		font-size: 0.72rem;
		font-weight: 600;
		letter-spacing: 0.04em;
		color: #4a6258;
	}
	.tag-sep {
		color: var(--c-sage);
		font-weight: 300;
	}

	.info-rule {
		width: 36px;
		height: 2px;
		background: linear-gradient(90deg, var(--c-green), var(--c-sage));
		border-radius: 2px;
		margin-bottom: 1.75rem;
	}

	.bio-text {
		font-size: 1rem;
		line-height: 1.72;
		color: #3d5047;
		max-width: 48ch;
		margin: 0 0 2.25rem;
		flex: 1;
	}

	/* CTA row */
	.cta-row {
		display: flex;
		align-items: center;
		gap: 0.85rem;
		flex-wrap: wrap;
	}

	.btn-primary {
		display: inline-flex;
		align-items: center;
		gap: 0.5rem;
		background: var(--c-dark);
		color: #fff;
		font-size: 0.8rem;
		font-weight: 600;
		letter-spacing: 0.02em;
		padding: 0.7rem 1.4rem;
		border-radius: 100px;
		border: none;
		cursor: pointer;
		transition:
			background 0.3s ease,
			transform 0.3s cubic-bezier(0.34, 1.56, 0.64, 1),
			box-shadow 0.3s ease;
		box-shadow: 0 4px 14px rgba(31, 36, 33, 0.22);
	}
	.btn-primary:hover {
		background: var(--c-teal);
		transform: scale(1.05);
		box-shadow: 0 8px 24px rgba(33, 104, 105, 0.32);
	}

	.link-pill {
		display: inline-flex;
		align-items: center;
		gap: 0.4rem;
		font-size: 0.75rem;
		font-weight: 600;
		color: #5a7265;
		text-decoration: none;
		padding: 0.55rem 1rem;
		border-radius: 100px;
		border: 1px solid var(--c-border);
		background: rgba(255, 255, 255, 0.5);
		transition:
			color 0.25s ease,
			background 0.25s ease,
			border-color 0.25s ease,
			transform 0.3s cubic-bezier(0.34, 1.56, 0.64, 1);
	}
	.link-pill:hover {
		color: var(--c-teal);
		background: rgba(255, 255, 255, 0.85);
		border-color: rgba(33, 104, 105, 0.3);
		transform: scale(1.04);
	}

	/* Decorative index */
	.deco-index {
		position: absolute;
		bottom: 1.5rem;
		right: 2rem;
		font-size: 0.6rem;
		font-weight: 600;
		letter-spacing: 0.14em;
		color: var(--c-mist);
		pointer-events: none;
		user-select: none;
	}
</style>
