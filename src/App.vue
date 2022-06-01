<script setup lang="ts">
const el = $ref < HTMLCanvasElement > ()
const ctx = $computed(() => el!.getContext('2d')!)
// TODO加绿叶和花
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
    length: 12,
    angle: 0,
  }
  step(b)
}

const pendingTasks: Function[] = []

function step(preBranch: Branch, depth = 0) {
  const startPint: Point = getEndPoint(preBranch)
  drawBranch(preBranch)
  if (depth < 3 || Math.random() < 0.5) {
    pendingTasks.push(() => step({
      startPint,
      length: preBranch.length + (Math.random() * 10 - 5),
      angle: preBranch.angle + (0.2 * Math.random()),
    }, depth + 1))
  }
  if (depth < 3 || Math.random() < 0.5) {
    pendingTasks.push(() => step({
      startPint,
      length: preBranch.length + (Math.random() * 10 - 5),
      angle: preBranch.angle - (0.2 * Math.random()),
    }, depth + 1))
  }
}

function frame() {
  const tasks = [...pendingTasks]
  pendingTasks.length = 0
  tasks.forEach(fn => fn())
}

let frameNumber = 0

function startFrame() {
  requestAnimationFrame(() => {
    frameNumber += 1
    if (frameNumber % 5 === 0)
      frame()
    startFrame()
  })
}

startFrame()

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

