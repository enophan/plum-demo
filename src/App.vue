<script setup lang="ts">
const el = ref < HTMLCanvasElement > ()
onMounted(() => {
  const canvas = el.value!
  const ctx = canvas.getContext('2d')!
})
</script>

<template>
  <div>
    <canvas ref="el" />
    <div>dnfsbfihdj</div>
  </div>
</template>
