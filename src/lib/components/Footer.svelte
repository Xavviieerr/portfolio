<script lang="ts">
	let email = '';
	let message = '';
	let name = '';
	let status: 'idle' | 'sending' | 'success' | 'error' = 'idle';

	async function submitForm() {
		if (status === 'sending') return;
		status = 'sending';

		try {
			const res = await fetch('/api/contact', {
				method: 'POST',
				headers: { 'Content-Type': 'application/json' },
				body: JSON.stringify({ name, email, message })
			});

			if (!res.ok) throw new Error('Request failed');

			status = 'success';
			name = '';
			email = '';
			message = '';

			// Reset after 4s
			setTimeout(() => {
				status = 'idle';
			}, 4000);
		} catch {
			status = 'error';
			setTimeout(() => {
				status = 'idle';
			}, 4000);
		}
	}
</script>

<footer class="footer">
	<div class="grain"></div>

	<!-- Ambient glow -->
	<div class="glow glow-l"></div>
	<div class="glow glow-r"></div>

	<div class="footer-inner">
		<!-- ── Top: CTA + Form ── -->
		<div class="footer-main">
			<!-- Left — headline -->
			<div class="footer-left">
				<span class="eyebrow">Open to opportunities</span>
				<h2 class="footer-heading">
					Ready to build<br />
					something<br />
					<em>exceptional?</em>
				</h2>
				<p class="footer-sub">
					Let's collaborate on products that feel fast, immersive, and intentional. I'm available
					for full-time roles, freelance, and open-source.
				</p>

				<!-- Social pills -->
				<div class="social-row">
					<a href="https://github.com/Xavviieerr" target="_blank" class="social-pill">
						<svg width="14" height="14" viewBox="0 0 14 14" fill="none"
							><path
								d="M7 1C3.686 1 1 3.686 1 7c0 2.656 1.727 4.904 4.124 5.698.3.055.41-.13.41-.29 0-.143-.006-.52-.008-1.022-1.671.363-2.024-.806-2.024-.806-.273-.694-.667-.88-.667-.88-.545-.373.041-.365.041-.365.603.042.92.619.92.619.536.918 1.406.653 1.748.499.054-.388.209-.653.38-.802-1.335-.152-2.738-.668-2.738-2.97 0-.656.234-1.193.619-1.613-.062-.152-.268-.763.058-1.59 0 0 .505-.162 1.655.617A5.98 5.98 0 017 4.98c.527.003 1.073.069 1.553.203 1.149-.779 1.653-.617 1.653-.617.327.827.121 1.438.059 1.59.386.42.619.957.619 1.613 0 2.308-1.405 2.816-2.744 2.965.217.187.409.555.409 1.115 0 .806-.007 1.455-.007 1.652 0 .161.109.349.414.29C11.275 11.902 13 9.655 13 7c0-3.314-2.686-6-6-6z"
								fill="currentColor"
							/></svg
						>
						GitHub
					</a>
					<a
						href="https://www.linkedin.com/in/chidera-paul-ogbu"
						target="_blank"
						class="social-pill"
					>
						<svg width="14" height="14" viewBox="0 0 14 14" fill="none"
							><path
								d="M2 1h10a1 1 0 011 1v10a1 1 0 01-1 1H2a1 1 0 01-1-1V2a1 1 0 011-1zm2.5 3a1 1 0 100 2 1 1 0 000-2zM3 6.5h3V12H3V6.5zm4 0h3v.75c.4-.55 1-.75 1.75-.75C13 6.5 13 8 13 9v3h-3V9.25c0-.45-.2-.75-.65-.75s-.85.3-.85.75V12H7V6.5z"
								fill="currentColor"
							/></svg
						>
						LinkedIn
					</a>
					<!-- <a href="#" target="_blank" class="social-pill">
						<svg width="14" height="14" viewBox="0 0 14 14" fill="none"
							><path
								d="M13 2.5s-1 .5-1.5.6A2.5 2.5 0 001.5 5c0 .3 0 .6.1.8C3.1 5.7 4.9 4.8 6 3.5c.2 1 0 2-.8 2.5A2.5 2.5 0 005 8.5c1 0 2-.5 2.5-1.5 0 1-.3 2-1.5 2.5A5 5 0 012 10.5c1.5 1 4 1.5 6-1 1.5-1.8 2-4 1.5-6L13 2.5z"
								fill="currentColor"
							/></svg
						>
						Twitter
					</a> -->
				</div>
			</div>

			<!-- Right — form -->
			<div class="footer-right">
				<div class="form-card">
					<p class="form-label">Send a message</p>

					<form on:submit|preventDefault={submitForm} novalidate>
						<div class="field">
							<label for="f-name">Name</label>
							<input
								id="f-name"
								type="text"
								bind:value={name}
								placeholder="Chioma Okafor"
								required
								disabled={status === 'sending' || status === 'success'}
							/>
						</div>

						<div class="field">
							<label for="f-email">Email</label>
							<input
								id="f-email"
								type="email"
								bind:value={email}
								placeholder="you@company.com"
								required
								disabled={status === 'sending' || status === 'success'}
							/>
						</div>

						<div class="field">
							<label for="f-msg">Message</label>
							<textarea
								id="f-msg"
								bind:value={message}
								placeholder="Tell me about the project, role, or idea…"
								rows="5"
								required
								disabled={status === 'sending' || status === 'success'}
							></textarea>
						</div>

						<button
							type="submit"
							class="submit-btn"
							class:sending={status === 'sending'}
							class:success={status === 'success'}
							class:error={status === 'error'}
							disabled={status === 'sending' || status === 'success'}
						>
							{#if status === 'idle'}
								<span>Send Message</span>
								<svg width="16" height="16" viewBox="0 0 16 16" fill="none"
									><path
										d="M3 8h10M9 4l4 4-4 4"
										stroke="currentColor"
										stroke-width="1.6"
										stroke-linecap="round"
										stroke-linejoin="round"
									/></svg
								>
							{:else if status === 'sending'}
								<span class="spinner"></span>
								<span>Sending…</span>
							{:else if status === 'success'}
								<svg width="16" height="16" viewBox="0 0 16 16" fill="none"
									><path
										d="M3 8l4 4 6-7"
										stroke="currentColor"
										stroke-width="1.8"
										stroke-linecap="round"
										stroke-linejoin="round"
									/></svg
								>
								<span>Message sent!</span>
							{:else}
								<span>Try again</span>
							{/if}
						</button>

						{#if status === 'error'}
							<p class="form-error">Something went wrong. Please try again or email directly.</p>
						{/if}
					</form>
				</div>
			</div>
		</div>

		<!-- ── Bottom bar ── -->
		<div class="footer-bottom">
			<p class="copyright">© {new Date().getFullYear()} Chidera Ogbu. Built with SvelteKit.</p>

			<div class="bottom-right">
				<a
					href="https://docs.google.com/document/d/18g25AVkfWyVb2TprTx7lHAgWntw5F5jSGDIA8VTLXtU/export?format=pdf"
					target="_blank"
					rel="noopener noreferrer"
					class="cv-btn"
				>
					<svg width="14" height="14" viewBox="0 0 14 14" fill="none"
						><path
							d="M7 1v8M7 9L4.5 6.5M7 9l2.5-2.5M2 12h10"
							stroke="currentColor"
							stroke-width="1.5"
							stroke-linecap="round"
							stroke-linejoin="round"
						/></svg
					>
					Download CV
				</a>
				<span class="status-dot">
					<span class="dot-pulse"></span>
					Available for work
				</span>
			</div>
		</div>
	</div>
</footer>

<style>
	/* ── Palette ── */
	:root {
		--c-dark: #1f2421;
		--c-teal: #216869;
		--c-green: #49a078;
		--c-sage: #9cc5a1;
		--c-mist: #dce1de;
	}

	/* ── Footer shell — carbon black ── */
	.footer {
		position: relative;
		width: 100%;
		background: #0c0e0d;
		overflow: hidden;
		font-family: 'DM Sans', 'Outfit', system-ui, sans-serif;
		isolation: isolate;
	}

	/* ── Grain ── */
	.grain {
		position: absolute;
		inset: 0;
		z-index: 1;
		pointer-events: none;
		opacity: 0.038;
		background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
		background-repeat: repeat;
		background-size: 200px;
	}

	/* ── Ambient glows ── */
	.glow {
		position: absolute;
		border-radius: 50%;
		filter: blur(120px);
		pointer-events: none;
		z-index: 0;
	}
	.glow-l {
		width: 500px;
		height: 400px;
		top: -80px;
		left: -100px;
		background: radial-gradient(ellipse, rgba(33, 104, 105, 0.22) 0%, transparent 70%);
	}
	.glow-r {
		width: 380px;
		height: 360px;
		bottom: -60px;
		right: -80px;
		background: radial-gradient(ellipse, rgba(73, 160, 120, 0.14) 0%, transparent 70%);
	}

	/* ── Inner wrapper ── */
	.footer-inner {
		position: relative;
		z-index: 2;
		max-width: 1200px;
		margin: 0 auto;
		padding: 6rem 2rem 2.5rem;
		display: flex;
		flex-direction: column;
		gap: 5rem;
	}

	/* ── Main grid ── */
	.footer-main {
		display: grid;
		grid-template-columns: 1fr 1fr;
		gap: 5rem;
		align-items: start;
	}

	/* ── Left ── */
	.eyebrow {
		display: inline-flex;
		align-items: center;
		gap: 0.5rem;
		font-size: 0.62rem;
		font-weight: 700;
		letter-spacing: 0.28em;
		text-transform: uppercase;
		color: var(--c-green);
		margin-bottom: 1.5rem;
	}
	.eyebrow::before {
		content: '';
		display: inline-block;
		width: 6px;
		height: 6px;
		border-radius: 50%;
		background: var(--c-green);
		box-shadow: 0 0 0 3px rgba(73, 160, 120, 0.2);
	}

	.footer-heading {
		font-size: clamp(2.6rem, 5vw, 4.2rem);
		font-weight: 800;
		letter-spacing: -0.04em;
		line-height: 1.05;
		color: var(--c-mist);
		margin: 0 0 1.5rem;
	}
	.footer-heading em {
		font-style: normal;
		color: var(--c-green);
		position: relative;
	}
	.footer-heading em::after {
		content: '';
		position: absolute;
		bottom: 3px;
		left: 0;
		right: 0;
		height: 2px;
		background: linear-gradient(90deg, var(--c-green), var(--c-sage));
		border-radius: 2px;
		opacity: 0.45;
	}

	.footer-sub {
		font-size: 0.9rem;
		line-height: 1.75;
		color: rgba(220, 225, 222, 0.5);
		max-width: 38ch;
		margin: 0 0 2rem;
	}

	/* Social pills */
	.social-row {
		display: flex;
		gap: 0.6rem;
		flex-wrap: wrap;
	}
	.social-pill {
		display: inline-flex;
		align-items: center;
		gap: 0.4rem;
		font-size: 0.72rem;
		font-weight: 600;
		letter-spacing: 0.04em;
		color: rgba(220, 225, 222, 0.55);
		text-decoration: none;
		padding: 0.45rem 0.9rem;
		border-radius: 999px;
		border: 1px solid rgba(220, 225, 222, 0.1);
		background: rgba(255, 255, 255, 0.04);
		transition:
			color 0.25s ease,
			border-color 0.25s ease,
			background 0.25s ease,
			transform 0.3s cubic-bezier(0.34, 1.56, 0.64, 1);
	}
	.social-pill:hover {
		color: var(--c-sage);
		border-color: rgba(156, 197, 161, 0.3);
		background: rgba(73, 160, 120, 0.08);
		transform: scale(1.04);
	}

	/* ── Right — form card ── */
	.form-card {
		background: rgba(255, 255, 255, 0.03);
		border: 1px solid rgba(220, 225, 222, 0.08);
		border-radius: 24px;
		padding: 2rem;
		box-shadow: 0 0 0 1px rgba(255, 255, 255, 0.02) inset;
	}
	.form-label {
		font-size: 0.62rem;
		font-weight: 700;
		letter-spacing: 0.24em;
		text-transform: uppercase;
		color: rgba(220, 225, 222, 0.3);
		margin: 0 0 1.5rem;
	}

	form {
		display: flex;
		flex-direction: column;
		gap: 1rem;
	}

	.field {
		display: flex;
		flex-direction: column;
		gap: 0.4rem;
	}
	.field label {
		font-size: 0.7rem;
		font-weight: 600;
		letter-spacing: 0.06em;
		color: rgba(220, 225, 222, 0.45);
	}

	input,
	textarea {
		background: rgba(255, 255, 255, 0.05);
		border: 1px solid rgba(220, 225, 222, 0.1);
		border-radius: 12px;
		padding: 0.8rem 1rem;
		color: var(--c-mist);
		font-family: inherit;
		font-size: 0.85rem;
		outline: none;
		resize: none;
		transition:
			border-color 0.25s ease,
			background 0.25s ease,
			box-shadow 0.25s ease;
	}
	input::placeholder,
	textarea::placeholder {
		color: rgba(220, 225, 222, 0.2);
	}
	input:focus,
	textarea:focus {
		border-color: rgba(73, 160, 120, 0.45);
		background: rgba(255, 255, 255, 0.07);
		box-shadow: 0 0 0 3px rgba(73, 160, 120, 0.08);
	}
	input:disabled,
	textarea:disabled {
		opacity: 0.5;
		cursor: not-allowed;
	}

	/* Submit button */
	.submit-btn {
		display: flex;
		align-items: center;
		justify-content: center;
		gap: 0.5rem;
		margin-top: 0.25rem;
		padding: 0.85rem 1.2rem;
		border-radius: 12px;
		border: none;
		background: var(--c-green);
		color: #fff;
		font-family: inherit;
		font-size: 0.82rem;
		font-weight: 700;
		letter-spacing: 0.04em;
		cursor: pointer;
		transition:
			background 0.3s ease,
			transform 0.3s cubic-bezier(0.34, 1.56, 0.64, 1),
			box-shadow 0.3s ease;
		box-shadow: 0 4px 20px rgba(73, 160, 120, 0.28);
	}
	.submit-btn:hover:not(:disabled) {
		background: var(--c-teal);
		transform: translateY(-2px) scale(1.015);
		box-shadow: 0 8px 28px rgba(33, 104, 105, 0.36);
	}
	.submit-btn:disabled {
		opacity: 0.7;
		cursor: not-allowed;
	}
	.submit-btn.success {
		background: rgba(73, 160, 120, 0.25);
		border: 1px solid rgba(73, 160, 120, 0.4);
		color: var(--c-sage);
		box-shadow: none;
	}
	.submit-btn.error {
		background: rgba(200, 60, 60, 0.25);
		color: #f4a0a0;
		box-shadow: none;
	}

	/* Spinner */
	.spinner {
		width: 14px;
		height: 14px;
		border: 2px solid rgba(255, 255, 255, 0.3);
		border-top-color: #fff;
		border-radius: 50%;
		animation: spin 0.7s linear infinite;
	}
	@keyframes spin {
		to {
			transform: rotate(360deg);
		}
	}

	.form-error {
		font-size: 0.72rem;
		color: #f4a0a0;
		margin: 0;
		text-align: center;
	}

	/* ── Bottom bar ── */
	.footer-bottom {
		display: flex;
		align-items: center;
		justify-content: space-between;
		border-top: 1px solid rgba(220, 225, 222, 0.07);
		padding-top: 1.5rem;
		flex-wrap: wrap;
		gap: 1rem;
	}
	.copyright {
		font-size: 0.7rem;
		color: rgba(220, 225, 222, 0.28);
		margin: 0;
		letter-spacing: 0.02em;
	}
	.bottom-right {
		display: flex;
		align-items: center;
		gap: 1.25rem;
	}

	.cv-btn {
		display: inline-flex;
		align-items: center;
		gap: 0.4rem;
		font-size: 0.72rem;
		font-weight: 600;
		letter-spacing: 0.06em;
		color: rgba(220, 225, 222, 0.55);
		text-decoration: none;
		padding: 0.45rem 0.9rem;
		border-radius: 999px;
		border: 1px solid rgba(220, 225, 222, 0.12);
		background: rgba(255, 255, 255, 0.04);
		transition:
			color 0.25s ease,
			border-color 0.25s ease,
			transform 0.3s cubic-bezier(0.34, 1.56, 0.64, 1);
	}
	.cv-btn:hover {
		color: var(--c-sage);
		border-color: rgba(156, 197, 161, 0.3);
		transform: scale(1.04);
	}

	.status-dot {
		display: inline-flex;
		align-items: center;
		gap: 0.45rem;
		font-size: 0.68rem;
		font-weight: 600;
		letter-spacing: 0.04em;
		color: rgba(220, 225, 222, 0.4);
	}
	.dot-pulse {
		width: 7px;
		height: 7px;
		border-radius: 50%;
		background: var(--c-green);
		box-shadow: 0 0 0 2px rgba(73, 160, 120, 0.2);
		animation: pulse 2.4s ease-in-out infinite;
	}
	@keyframes pulse {
		0%,
		100% {
			box-shadow: 0 0 0 2px rgba(73, 160, 120, 0.2);
		}
		50% {
			box-shadow: 0 0 0 6px rgba(73, 160, 120, 0.06);
		}
	}

	/* ── Responsive ── */
	@media (max-width: 900px) {
		.footer-main {
			grid-template-columns: 1fr;
			gap: 3rem;
		}
		.footer-inner {
			padding: 4rem 1.5rem 2rem;
			gap: 3rem;
		}
	}
	@media (max-width: 540px) {
		.footer-heading {
			font-size: 2.4rem;
		}
		.footer-bottom {
			flex-direction: column;
			align-items: flex-start;
		}
	}
</style>
