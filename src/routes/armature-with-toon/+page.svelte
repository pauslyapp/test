<script lang="ts">
  import { Canvas, T } from '@threlte/core'
  import { OrbitControls } from '@threlte/extras'
  import type { ComponentProps } from 'svelte'
  import Armature from './Pauslyactions.svelte'

  let actions: ComponentProps<Armature>['actions']

  $: console.log($actions)
  // $: $actions?.PoseLib?.play()

  const stopAll = () => {
    $actions?.Squats?.stop()
    $actions?.Squats2?.stop()
    $actions?.Squats3?.stop()
    $actions?.Deadlifts?.stop()
    $actions?.PoseLib?.stop()
  }
  const playSquats = () => {
    stopAll()
    $actions?.Squats?.play()
  }
  const playSquats2 = () => {
    stopAll()
    $actions?.Squats2?.play()
  }
  const playSquats3 = () => {
    stopAll()
    $actions?.Squats3?.play()
  }
  const playDeadlifts = () => {
    stopAll()
    $actions?.Deadlifts?.play()
  }
  // const
  let controls
</script>

<div class="canvas">
  <Canvas>
    <T.PerspectiveCamera makeDefault position={[0, 3, 5]} fov={20}>
      <OrbitControls enableZoom={true} target={[0, 0.9, 0]} enableDamping />
    </T.PerspectiveCamera>
    <!-- <T.MeshToonMaterial color={[255, 0, 0]} />-->

    <T.DirectionalLight position={[10, 5, 5]} intensity={4} />
    <Armature bind:actions />

    <!-- <Armature /> -->
  </Canvas>
</div>

<button on:click={playSquats}>Squats</button>
<button on:click={playSquats2}>Squats 2</button>
<button on:click={playSquats3}>Squats 3</button>
<button on:click={playDeadlifts}>Deadlifts</button>
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
