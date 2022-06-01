<script setup lang="ts">
const el = $ref < HTMLCanvasElement > ()
const ctx = $computed(() => el!.getContext('2d')!)

const WIDTH = 500
const HEIGHT = 500

function init() {
  ctx.strokeStyle = '#fff' // stghyle shouold on the top
  Line(WIDTH / 2, HEIGHT, WIDTH / 2, HEIGHT / 2)
}

function Line(x1: number, y1: number, x2: number, y2: number) {
  ctx.beginPath() // Start a new path
  ctx.moveTo(x1, y1) // Move the pen to (30, 50)
  ctx.lineTo(x2, y2) // Draw a line to (150, 100)
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

