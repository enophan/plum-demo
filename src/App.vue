<script setup lang="ts">
const el = $ref < HTMLCanvasElement > ()
const ctx = $computed(() => el!.getContext('2d')!)

const WIDTH = 500
const HEIGHT = 500

interface Point {
  x: number
  y: number
}

function init() {
  ctx.strokeStyle = '#fff' // stghyle shouold on the top
  const startPoint: Point = { x: WIDTH / 2, y: HEIGHT }
  const endPoint: Point = { x: WIDTH / 2, y: HEIGHT / 2 }
  Line(startPoint, endPoint)
}

function Line(p1: Point, p2: Point) {
  ctx.beginPath() // Start a new path
  ctx.moveTo(p1.x, p1.y) // Move the pen to (x1, y1)
  ctx.lineTo(p2.x, p2.y) // Draw a line to (x2, y2)
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

