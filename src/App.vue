<script setup lang="ts">
const el = $ref < HTMLCanvasElement > ()
const ctx = $computed(() => el!.getContext('2d')!)
// TODO 加绿叶和花
// TODO 解决树枝向下长的问题
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
  ctx.strokeStyle = 'rgba(156,163,175,0.5)'
  const b: Branch = {
    startPint: { x: WIDTH / 2, y: HEIGHT },
    length: 16,
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
    <canvas ref="el" width="500" height="500" border="3 gray-400/50" m-100px />
  </div>
</template>

