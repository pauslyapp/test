<script>
  import { T, useParent, forwardEventHandlers, useRender, useFrame } from '@threlte/core'
  export let thresholdAngle = undefined
  export let color = undefined
  const parent = useParent()
  if (!$parent || ($parent.type !== 'Mesh' && $parent.type !== 'SkinnedMesh'))
    throw new Error('Edges: component must be a child of a Mesh')
  const parentMesh = $parent
  let geometry = 'clone' in parentMesh.geometry ? parentMesh.geometry.clone() : parentMesh.geometry
  const component = forwardEventHandlers()
  useFrame((context) => {
    //
    geometry = 'clone' in parentMesh.geometry ? parentMesh.geometry.clone() : parentMesh.geometry
  })
</script>

<T.LineSegments let:ref {...$$restProps} bind:this={$component}>
  <T.EdgesGeometry args={[geometry, thresholdAngle]} />
  <T.LineBasicMaterial {color} />
  <slot {ref} />
</T.LineSegments>
