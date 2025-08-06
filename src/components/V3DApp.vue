<template>
  <div :id="containerId">
    <div :id="fsButtonId" class="fullscreen-button fullscreen-open" title="Toggle fullscreen mode"></div>
    <div class="changeColor">
      <button @click="changeColor">Change Background Color</button>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { ref, onMounted } from 'vue'
import { createApp } from '@/v3dApp/app'
import * as v3d from 'verge3d';


const containerId = ref('')

const app = ref(null)

const PL = ref(null)

const fsButtonId = ref('')

const uuid = ref('')

const sceneURL = 'v3dApp/app.gltf'

const logicURL = 'v3dApp/visual_logic.js'

const loadApp = async () => {
  app.value = null
  PL.value = null
  const uuidData: string = window.crypto.randomUUID();
  uuid.value = uuidData
  containerId.value = `v3d-container-${uuidData}`
  fsButtonId.value = `fullscreen-button-${uuidData}`
  const result = await createApp({
    containerId: containerId.value,
    fsButtonId: fsButtonId.value,
    sceneURL,
    logicURL,
  })
  if (result) {
    app.value = result.app
    PL.value = result.PL
  } else {
    console.error('Failed to create V3DApp')
  }
  console.log('result', result)
}

const changeColor = () => {
  if (app.value && app.value.scene) {
    app.value.scene.background = new v3d.Color("rgb(0, 255, 0)");
  }
}

onMounted(() => {
  loadApp()
})

</script>

<style>
@import '@/v3dApp/app.css';

.changeColor {
  position: absolute;
  top: 10px;
  left: 10px;
  z-index: 1000;
}
</style>
