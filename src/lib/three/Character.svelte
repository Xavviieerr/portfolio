<script lang="ts">
	import { T, useTask } from '@threlte/core';
	import { useGltf, useGltfAnimations } from '@threlte/extras';
	import * as THREE from 'three';

	// Load the GLTF model
	const gltf = useGltf('/models/Housedancing.glb');

	// Auto-play the first animation (salute / dance) if one exists
	const { actions } = useGltfAnimations(gltf);

	// Play first available animation as soon as actions load
	$effect(() => {
		if ($actions) {
			const first = Object.values($actions)[0];
			if (first && !first.isRunning()) {
				first.reset().fadeIn(0.4).play();
			}
		}
	});

	// Gentle auto-rotation on Y axis for presence
	let meshRef: THREE.Group | undefined;
	let t = 0;

	useTask((delta) => {
		if (!meshRef) return;
		t += delta;
		// Slow idle sway — ±5 degrees, period ~6s
		meshRef.rotation.y = Math.sin(t * 0.5) * 0.08;
	});
</script>

{#if $gltf?.scene}
	<!--
		Scale rationale:
		Most Mixamo/Sketchfab humanoids are exported at ~1.7–2m real-world scale.
		With a camera at z=5, fov ~50, a scale of 1 and position y=-1 puts
		the figure nicely framed — head near top 1/3, feet at bottom.

		If your model looks tiny → increase scale (try 1.4).
		If your model looks massive → decrease scale (try 0.7).
		Adjust position.y to sink/raise the figure relative to the camera.
	-->
	<T
		is={$gltf.scene}
		bind:ref={meshRef}
		scale={[0.014, 0.014, 0.014]}
		position={[0, -1.0, 0]}
		rotation={[0, 0.15, 0]}
		castShadow
		receiveShadow
	/>
{:else}
	<!-- Fallback placeholder while model loads — a simple capsule stand-in -->
	<T.Mesh position={[0, 0, 0]} castShadow>
		<T.CapsuleGeometry args={[0.35, 1.4, 4, 16]} />
		<T.MeshStandardMaterial color="#216869" wireframe />
	</T.Mesh>
{/if}
