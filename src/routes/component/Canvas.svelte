<script lang="ts">
	import { Canvas, T } from '@threlte/core'
	import { Edges, OrbitControls } from '@threlte/extras'
	import Scene from './Scene.svelte'
</script>

<Canvas>
	<T.MeshToonMaterial color={[255, 0, 0]} />

	<T.PerspectiveCamera makeDefault position={[0, 3, 5]} fov={20}>
		<OrbitControls enableZoom={true} target={[0, 0, 0]} />
	</T.PerspectiveCamera>
	<T.DirectionalLight position={[10, 5, 5]} intensity={3} />

	<Scene />
</Canvas>
