<script lang="ts">
	import { Canvas, T } from '@threlte/core';
	import { Float, ContactShadows, Environment } from '@threlte/extras';
	import Character from './Character.svelte';

	// Expose a prop so the parent can adjust vertical fill
	let { height = '100%' } = $props();
</script>

<div class="scene-shell" style="height:{height}">
	<Canvas shadows>
		<!--
			CAMERA
			─────
			fov 45 gives a natural portrait look (not fish-eye).
			position z=4.2 with the character at z=0 frames a full-body shot.
			Looking slightly upward (y=0.8 target) creates a heroic angle.
		-->
		<T.PerspectiveCamera makeDefault fov={45} near={0.1} far={100} position={[0, 0.8, 4.2]}>
			<T.OrthographicCamera />
		</T.PerspectiveCamera>

		<!--
			LIGHTING RIG — dark mystery theme
			───────────────────────────────────
			1. Dim ambient so shadows stay deep
			2. Key light: warm top-right (hero lighting classic)
			3. Rim light: cool teal from behind-left (palette)
			4. Fill light: very dim, prevents pure black voids
		-->

		<!-- Ambient — intentionally low for drama -->
		<T.AmbientLight intensity={0.25} color="#1a2420" />

		<!-- Key light — warm, top right -->
		<T.DirectionalLight
			castShadow
			position={[3, 6, 4]}
			intensity={2.2}
			color="#ffe8c8"
			shadow.camera.near={0.5}
			shadow.camera.far={50}
			shadow.camera.left={-4}
			shadow.camera.right={4}
			shadow.camera.top={6}
			shadow.camera.bottom={-4}
			shadow.mapSize.width={2048}
			shadow.mapSize.height={2048}
			shadow.bias={-0.001}
		/>

		<!-- Rim light — teal from behind, creates the glowing edge -->
		<T.DirectionalLight position={[-3, 2, -5]} intensity={1.8} color="#49a078" />

		<!-- Fill — stops the shadow side going jet black -->
		<T.DirectionalLight position={[0, -1, 3]} intensity={0.3} color="#9cc5a1" />

		<!-- Point light at foot level — pools like a stage spotlight -->
		<T.PointLight position={[0, -0.8, 2]} intensity={1.2} color="#216869" distance={6} decay={2} />

		<!-- CHARACTER with gentle float -->
		<Float speed={0.9} rotationIntensity={0.08} floatIntensity={0.25}>
			<Character />
		</Float>

		<!--
			CONTACT SHADOW
			Pools under the feet — crucial for grounding the character.
			scale=6 covers the foot spread, blur=3 softens it naturally.
		-->
		<ContactShadows
			position={[0, -1.06, 0]}
			scale={6}
			blur={3.5}
			far={2}
			opacity={0.55}
			color="#0a1410"
		/>

		<!--
			ENVIRONMENT
			'night' or a dark neutral. No env file needed — uses built-in presets.
			We keep it subtle (backgroundIntensity low) so lighting rig dominates.
		-->
		<!-- <Environment backgroundIntensity={0.15} /> -->
	</Canvas>

	<!-- Gradient vignette overlay — fades the 3D into the page on edges -->
	<div class="vignette"></div>

	<!-- Bottom fog — blends feet with page background seamlessly -->
	<div class="bottom-fade"></div>
</div>

<style>
	.scene-shell {
		position: relative;
		width: 100%;
		overflow: hidden;
	}

	/* Radial vignette softens the canvas edges into the dark background */
	.vignette {
		position: absolute;
		inset: 0;
		pointer-events: none;
		background: radial-gradient(ellipse at 50% 50%, transparent 45%, rgba(10, 14, 12, 0.55) 100%);
		z-index: 2;
	}

	/* Bottom fog dissolves feet/ground shadow into page */
	.bottom-fade {
		position: absolute;
		bottom: 0;
		left: 0;
		right: 0;
		height: 30%;
		background: linear-gradient(to top, #0a0e0c, transparent);
		pointer-events: none;
		z-index: 3;
	}
</style>
