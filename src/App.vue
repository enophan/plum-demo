<script setup lang="ts">
const el = $ref < HTMLCanvasElement > ()
const ctx = $computed(() => el!.getContext('2d')!)

function init() {
  ctx.strokeStyle = '#fff' // style shouold on the top
}

function Line(x1: number, y1: number, x2: number, y2: number) {
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

