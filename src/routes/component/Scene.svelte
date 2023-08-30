<script lang="ts">
	import { Canvas, T, extend, useRender } from '@threlte/core'
	import { Edges, OrbitControls } from '@threlte/extras'
	import ConditionalEdges from './lib/ConditionalEdges.svelte'
	import Scene from './Scene.svelte'
	import { TorusKnotGeometry } from 'three'
	import { OutlineEffect } from 'postprocessing'

	// extend({ OutlineEffect })

	// useRender(({ renderer,  }) => {

	// })
</script>

<T.Mesh position.y={0}>
	<T.BoxGeometry args={[1, 1, 1]} />
	<T.TorusKnotGeometry args={[0.5, 0.15, 64 * 4, 16 * 2]} />
	<!-- <T.TetrahedronGeometry args={[0.5]} /> -->
	<T.MeshToonMaterial color="hotpink" />

	<!-- <Edges thresholdAngle={15} color="black"  /> -->
	<ConditionalEdges color="black" />
	<!-- <T.OutlineEffect /> -->
</T.Mesh>
