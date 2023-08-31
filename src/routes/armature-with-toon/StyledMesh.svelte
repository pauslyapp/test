<script lang="ts">
  import { T } from '@threlte/core'
  import type * as THREE from 'three'
  import ConditionalEdges from '../component/lib/ConditionalEdges.svelte'

  export let mesh: THREE.Mesh
</script>

<T.Mesh geometry={mesh.geometry}>
  <T.MeshToonMaterial color="hotpink" />
  <ConditionalEdges color="black" />
</T.Mesh>
