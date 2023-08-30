<script lang="ts">
	import type * as THREE from 'three'
	import { useThrelte, useRender } from '@threlte/core'
	import {
		EffectComposer,
		EffectPass,
		RenderPass,
		OutlineEffect,
		BlendFunction,
		BloomEffect,
		KernelSize
	} from 'postprocessing'
	export let selectedMesh: THREE.Mesh
	const { scene, renderer, camera, size } = useThrelte()
	const composer = new EffectComposer(renderer)

	const setupEffectComposer = (camera: THREE.Camera, selectedMesh: THREE.Mesh) => {
		composer.removeAllPasses()
		composer.addPass(new RenderPass(scene, camera))
		const outlineEffect = new OutlineEffect(scene, camera, {
			blendFunction: BlendFunction.ALPHA,
			edgeStrength: 10,
			pulseSpeed: 0.0,
			visibleEdgeColor: 0x000000,
			xRay: false,
			blur: true,
			kernelSize: KernelSize.VERY_SMALL
		})
		scene.getObjectsByProperty('type', 'Mesh').forEach((mesh) => {
			outlineEffect.selection.add(mesh)
		})
		scene.overrideMaterial
		if (selectedMesh !== undefined) {
			// outlineEffect.selection.add(scene)
			outlineEffect.selection.add(selectedMesh)
		}
		composer.addPass(new EffectPass(camera, outlineEffect))
		// composer.addPass(
		// 	new EffectPass(
		// 		camera,
		// 		new BloomEffect({
		// 			intensity: 1,
		// 			luminanceThreshold: 0.15,
		// 			height: 512,
		// 			width: 512,
		// 			luminanceSmoothing: 0.08,
		// 			mipmapBlur: true,
		// 			kernelSize: KernelSize.MEDIUM
		// 		})
		// 	)
		// );
	}
	$: setupEffectComposer($camera, selectedMesh)
	$: composer.setSize($size.width, $size.height)
	useRender((_, delta) => {
		composer.render(delta)
	})
</script>

<!-- <Scene /> -->
