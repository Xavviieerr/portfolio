<script lang="ts">
	import { T } from '@threlte/core';
</script>

<T.AmbientLight intensity={0.5} />

<T.DirectionalLight position={[3, 5, 2]} intensity={1} />

<T.Mesh>
	<T.BoxGeometry args={[1, 1, 1]} />
	<T.MeshStandardMaterial color="white" />
</T.Mesh>
