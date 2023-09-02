<script lang="ts">
  import { Canvas, T, useLoader } from '@threlte/core'
  import { ContactShadows, OrbitControls } from '@threlte/extras'
  import type { ComponentProps } from 'svelte'
  import Armature from './New.svelte'
  import Effects from './Effects.svelte'

  let actions: ComponentProps<Armature>['actions']

  $: console.log($actions)
  // $: $actions?.PoseLib?.play()

  const stopAll = () => {
    $actions?.squats?.stop()
    $actions?.['push-ups']?.stop()
    $actions?.['Parenting Pose']?.stop()
  }
  const playSquats = () => {
    stopAll()
    $actions?.squats?.play()
  }
  const playPushUps = () => {
    stopAll()
    $actions?.['push-ups']?.play()
  }
  // const
  let controls
</script>

<div class="canvas">
  <Canvas toneMapping={0}>
    <Effects />
    <T.PerspectiveCamera makeDefault position={[0, 3, 5]} fov={20}>
      <OrbitControls enableZoom={true} target={[0, 0.9, 0]} enableDamping />
    </T.PerspectiveCamera>
    <!-- <T.MeshToonMaterial color={[255, 0, 0]} />-->

    <T.DirectionalLight position={[-10, 5, 5]} intensity={3.5} />
    <Armature bind:actions />

    <ContactShadows scale={2} blur={1} far={1} opacity={0.1} />
    <!-- <Armature /> -->
  </Canvas>
</div>

<button on:click={playSquats}>Squats</button>
<button on:click={playPushUps}>Push Ups</button>
<button on:click={stopAll}>Stop</button>

<style lang="postcss">
  .canvas {
    width: 40rem;
    height: 40rem;
    max-width: 100%;
    max-height: 80vh;
    border: 1px solid black;
  }
</style>
