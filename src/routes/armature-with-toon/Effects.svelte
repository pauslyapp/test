<script>
  import { useRender, useThrelte } from '@threlte/core'
  import { OutlineEffect } from 'three/examples/jsm/effects/OutlineEffect'
  const { scene, renderer, camera, size } = useThrelte()
  // To use the EffectComposer we need to pass arguments to the
  // default WebGLRenderer: https://github.com/pmndrs/postprocessing#usage

  useRender((_, delta) => {
    const outlineEffect = new OutlineEffect(renderer, {
      //
      defaultThickness: 0.0035,
      defaultColor: [0.05, 0.1, 0.1],
      // defaultThickness: 0.001,
      //
    })
    outlineEffect.render(scene, $camera)

    // composer.render(delta)
  })
</script>
