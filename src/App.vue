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
  length: number
  angle: number
}

function init() {
  ctx.strokeStyle = '#fff'
  const b: Branch = {
    startPint: { x: WIDTH / 2, y: HEIGHT },
    length: 100,
    angle: Math.PI / 24,
  }
  drawBranch(b)
  step(b)
}

function step(preBranch: Branch) {
  const startPint: Point = getEndPoint(preBranch)
  const leftBranch: Branch = {
    startPint,
    length: 100,
    angle: preBranch.angle + 0.1,
  }
  const rightBranch: Branch = {
    startPint,
    length: 100,
    angle: preBranch.angle - 0.1,
  }
  drawBranch(leftBranch)
  drawBranch(rightBranch)
}

function drawLine(startPoint: Point, endPoint: Point) {
  ctx.beginPath()
  ctx.moveTo(startPoint.x, startPoint.y)
  ctx.lineTo(endPoint.x, endPoint.y)
  ctx.stroke()
}

function getEndPoint(branch: Branch): Point {
  return {
    x: branch.startPint.x + branch.length * Math.sin(branch.angle),
    y: branch.startPint.y - branch.length * Math.cos(branch.angle),
  }
}

function drawBranch(barnch: Branch) {
  drawLine(barnch.startPint, getEndPoint(barnch))
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

