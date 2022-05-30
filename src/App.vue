<script setup lang="ts">
const el = ref < HTMLCanvasElement > ()
onMounted(() => {
  const canvas = el.value!
  const ctx = canvas.getContext('2d')!

  ctx.beginPath() // Start a new path
  ctx.moveTo(30, 50) // Move the pen to (30, 50)
  ctx.lineTo(150, 100) // Draw a line to (150, 100)
  ctx.stroke() // render the line
})
</script>

<template>
  <div flex="~" items-center justify-center>
    <canvas ref="el" width="400" height="400" border />
  </div>
</template>

