<script lang="ts">
	import { T, useParent, forwardEventHandlers, extend } from '@threlte/core'
	import { ShaderMaterial, type Mesh, LineSegments, BufferGeometry, EdgesGeometry } from 'three'
	import type { EdgesEvents, EdgesProps, EdgesSlots } from './ConditionalEdges.svelte'
	import { OutsideEdgesGeometry } from './src/OutsideEdgesGeometry.js'
	import { ConditionalEdgesGeometry } from './src/ConditionalEdgesGeometry.js'
	import { ConditionalEdgesShader } from './src/ConditionalEdgesShader.js'
	import { ConditionalLineSegmentsGeometry } from './src/Lines2/ConditionalLineSegmentsGeometry.js'
	import { ConditionalLineMaterial } from './src/Lines2/ConditionalLineMaterial.js'
	import { ColoredShadowMaterial } from './src/ColoredShadowMaterial.js'
	import * as BufferGeometryUtils from 'three/examples/jsm/utils/BufferGeometryUtils.js'
	import { LineSegmentsGeometry } from 'three/examples/jsm/lines/LineSegmentsGeometry.js'
	import { LineSegments2 } from 'three/examples/jsm/lines/LineSegments2.js'
	import { LineMaterial } from 'three/examples/jsm/lines/LineMaterial.js'

	type $$Props = EdgesProps
	type $$Events = EdgesEvents
	type $$Slots = EdgesSlots

	export let thresholdAngle: $$Props['thresholdAngle'] = undefined as $$Props['thresholdAngle']
	export let color: $$Props['color'] = undefined as $$Props['color']

	const parent = useParent()
	if (!$parent || $parent.type !== 'Mesh')
		throw new Error('Edges: component must be a child of a Mesh')

	const parentMesh = $parent as Mesh

	const conditionalMesh = parentMesh.clone()

	let lineGeom: ConditionalEdgesGeometry
	let thickLineGeom: ConditionalEdgesGeometry
	let material: ShaderMaterial
	let thickLines: LineSegments2

	let edgesGeom: EdgesGeometry
	let thickEdgesGeom: LineSegmentsGeometry

	const LIGHT_BACKGROUND = 0xeeeeee
	const LIGHT_MODEL = 0xffffff
	const LIGHT_LINES = 0x455a64
	const LIGHT_SHADOW = 0xc4c9cb

	const DARK_BACKGROUND = 0x111111
	const DARK_MODEL = 0x111111
	const DARK_LINES = 0xb0bec5
	const DARK_SHADOW = 0x2c2e2f
	const initGeometry = () => {
		const meshes: Mesh[] = []
		conditionalMesh.traverse((c) => {
			if (c.type === 'Mesh') {
				meshes.push(c as Mesh)
			}
		})
		for (const key in meshes) {
			const mesh = meshes[key]
			const parent = mesh.parent

			// Remove everything but the position attribute
			const mergedGeom = mesh.geometry.clone()
			for (const key in mergedGeom.attributes) {
				if (key !== 'position') {
					mergedGeom.deleteAttribute(key)
				}
			}

			edgesGeom = new EdgesGeometry(mesh.geometry, 25)

			thickEdgesGeom = new LineSegmentsGeometry().fromEdgesGeometry(edgesGeom)

			// Create the conditional edges geometry and associated material
			lineGeom = new ConditionalEdgesGeometry(BufferGeometryUtils.mergeVertices(mergedGeom))

			thickLineGeom = new ConditionalLineSegmentsGeometry().fromConditionalEdgesGeometry(lineGeom)
			// thickLines = new LineSegments2(
			// 	thickLineGeom as unknown as LineSegmentsGeometry,
			// 	new ConditionalLineMaterial({ color: LIGHT_LINES, linewidth: 2 }) as unknown as LineMaterial
			// )
			// thickLines.position.copy(mesh.position)
			// thickLines.scale.copy(mesh.scale)
			// thickLines.rotation.copy(mesh.rotation)

			conditionalMesh.remove(mesh)
			// conditionalMesh.add(line)
			// conditionalMesh.add(thickLines)
		}
	}
	initGeometry()

	extend({
		LineSegments2,
		LineMaterial
	})

	const thickLineMaterial = new ConditionalLineMaterial({
		color: 'black',
		linewidth: 0.005
	}) as unknown as LineMaterial

	material = new ShaderMaterial(ConditionalEdgesShader)
	material.uniforms.diffuse.value.set(LIGHT_LINES)

	const component = forwardEventHandlers()
</script>

<!-- Thick lines -->
<T.LineSegments2 geometry={thickLineGeom} material={thickLineMaterial}>
	<!-- position={parentMesh.position}
	scale={parentMesh.scale}
	rotation={parentMesh.rotation} -->
	<!-- <T.LineMaterial material={thickLineMaterial} /> -->
</T.LineSegments2>
<!-- <T.LineSegments geometry={lineGeom} {material} /> -->

<!-- Edges -->

<!-- Thick -->
<T.LineSegments2 geometry={thickEdgesGeom} material={thickLineMaterial}>
	<!-- <T.LineMaterial color={LIGHT_LINES} linewidth={10} /> -->
</T.LineSegments2>

<!-- <T.LineSegments geometry={edgesGeom}>
	<T.LineBasicMaterial color={0x000000} />
</T.LineSegments> -->

<!-- <T.LineSegments let:ref {...$$restProps} bind:this={$component} geometry={conditionalMesh}>
	<T.EdgesGeometry  />
	<T.LineBasicMaterial color={0x000000} /> -->
<!-- <T.LineBasicMaterial {material} /> -->
<!-- <slot {ref} />
</T.LineSegments> -->
