<script setup lang="ts">
const el = ref < HTMLCanvasElement > ()

function init() {
  const canvas = el.value!
  const ctx = canvas.getContext('2d')!

  ctx.strokeStyle = '#fff' // style on the top
  ctx.beginPath() // Start a new path
  ctx.moveTo(30, 50) // Move the pen to (30, 50)
  ctx.lineTo(150, 100) // Draw a line to (150, 100)
  ctx.stroke() // render the line
}

onMounted(() => {
  init()
})
</script>

<template>
  <div flex="~" items-center justify-center>
    <canvas ref="el" width="500" height="500" border />
  </div>
</template>

