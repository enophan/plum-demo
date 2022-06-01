<script setup lang="ts">
const el = $ref < HTMLCanvasElement > ()
const ctx = $computed(() => el!.getContext('2d')!)

const WIDTH = 500
const HEIGHT = 500

interface Point {
  x: number
  y: number
}

interface Branch {
  startPint: Point
  lenth: number
  angle: number
}

function init() {
  ctx.strokeStyle = '#fff' // stghyle shouold on the top
  const b: Branch = {
    startPint: { x: WIDTH / 2, y: HEIGHT },
    lenth: 100,
    angle: Math.PI / 6,
  }
  stratAndEnd(b)
}

function growingBranch(p1: Point, p2: Point) {
  ctx.beginPath() // Start a new path
  ctx.moveTo(p1.x, p1.y) // Move the pen to (x1, y1)
  ctx.lineTo(p2.x, p2.y) // Draw a line to (x2, y2)
  ctx.stroke() // render the line
}

function stratAndEnd(b: Branch) {
  const { startPint, lenth, angle } = b
  const endPoint: Point = {
    x: startPint.x + lenth * Math.sin(angle),
    y: startPint.y - lenth * Math.cos(angle),
  }
  growingBranch(startPint, endPoint)
}

onMounted(() => {
  init()
})
</script>

<template>
  <div flex="~" items-center justify-center>
    <canvas ref="el" width="500" height="500" border m-100px />
  </div>
</template>

