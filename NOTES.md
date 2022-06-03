## 绑定

```vue
<script setup lang="ts">
const el = ref < HTMLCanvasElement > ()
</script>

<template>
  <div>
    <canvas ref="el" width="400" height="400" />
  </div>
</template>
```

`onMounted`：Register a callback to be called after the component has been mounted.Param is a callback function.（注册一个回调函数，用以组件安装后调用。参数是回调函数）

the `<canvas>` element only has `width` and `height`, both of two attributes.

The id attribute is not personal attribute of the `<canvas>` , it is one of the global HTML attributes and any HTML elements all can apply it.

`getContext()`: this function is used to obtain the rendering context and drawing function of the `<canvas>` element. It takes one parameter, the type of context, like 2d and 3d.这个函数获取`<canvas>`的渲染内容和绘画功能。函数的参数是上下文类型，比如2d和3d。

In js

```js
const canvas = document.getElementById('tutorial')
const ctx = canvas.getContext('2d')
```

In vue

```typescript
const el = ref<HTMLCanvasElement>()
const canvas = el.value
const ctx = canvas.getContext('2d')
```

```vue
<script setup lang="ts">
const el = ref < HTMLCanvasElement > ()
onMounted(() => {
  const canvas = el.value!
  const ctx = canvas.getContext('2d')!
})
</script>
```

## canvas

```js
ctx.beginPath() // Start a new path
ctx.moveTo(30, 50) // Move the pen to (30, 50)
ctx.lineTo(150, 100) // Draw a line to (150, 100)
ctx.stroke() // render the line
```

`<canvas>` element's origin on the top-left, and the x-axis is from top-left to top-rigth, the y-axis is from top-left to bottom-left.

For canvas the tree's branch is from one point to another point, but actually the growing tree starts at the end of the previous branch, offsets an angle, and then grows to another location.

```typescript
interface Branch {
  startPint: Point
  lenth: number
  angle: number
}
```

When I followed Anthony Fu to code, there is a point that's been bathering me, where is the `angle` actully. Is the `angle` between the x-axis and the branch or between the y-axis and the branch? I have no idea.

Now I know that's stupid. Beacuse the angle is you defined, it could between the x-axis and the branch or between the y-axis and the branch, you just define it and note the change of symbols.

在`Branch`里定义的`angle`在canvas里并不具体的表示某个夹角，只是一个参数而已。最终会加上`lenth * Math.sin(angle)`，当然也可以减去。具体看要从哪里生长这棵树，要是从底部生长，

<img src="https://enophan-picgo-core.oss-cn-hangzhou.aliyuncs.com/enophan.github.io/Snipaste_2022-06-01_15-35-00.jpg" style="zoom:50%;" />

按照这个非常规的坐标系，为了防止超出画布（白框），就需要考虑`angle`的正负和`lenth * Math.sin(angle)`、`lenth * Math.cos(angle)`正负了。反正最后的结果要让加在y坐标上的为负数，加在x上的正负均可，负数则向右偏移，整数则向左偏移。不过，有些人偏向具象思维，那我们假定

<img src="https://enophan-picgo-core.oss-cn-hangzhou.aliyuncs.com/enophan.github.io/Snipaste_2022-06-01_15-44-23.jpg" style="zoom:50%;" />

这个θ就是`angle`，这也符合正常思维。那么就应该这样

```typescript
const b: Branch = {
    startPint: { x: WIDTH / 2, y: HEIGHT },
    lenth: 100,
    angle: Math.PI / 6,
  }

const endPoint: Point = {
    x: startPint.x + lenth * Math.sin(angle),
    y: startPint.y - lenth * Math.cos(angle),
  }
```

让y加的是负的，也就是减去`lenth * Math.cos(angle)`

## 接下来生成第二根树枝

当然是递归，但因为我现在还没到写递归能一气呵成的地步，所以先一步一步来。

```typescript
function init() {
  ctx.strokeStyle = '#fff'
  const b: Branch = {
    startPint: { x: WIDTH / 2, y: HEIGHT },
    length: 100,
    angle: Math.PI / 24,
  }
  stratAndEnd(b)
}
```

这是第一根枝，那么第二根枝就应该这样

```typescript
function init() {
  ctx.strokeStyle = '#fff'
  const b: Branch = {
    startPint: { x: WIDTH / 2, y: HEIGHT },
    length: 100,
    angle: Math.PI / 24,
  }
  stratAndEnd(b)
  const leftBranch: Branch = {
    startPint: getEndBranch(b),
    length: 100,
    angle: Math.PI / 12,
  }
  stratAndEnd(leftBranch)
}
```

重构一下

```typescript
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
```

开始写递归

```typescript
function init() {
  ctx.strokeStyle = 'rgba(156,163,175,0.5)'
  const b: Branch = {
    startPint: { x: WIDTH / 2, y: HEIGHT },
    length: 10,
    angle: 0,
  }
  step(b)
}

function step(preBranch: Branch) {
  const startPint: Point = getEndPoint(preBranch)
  drawBranch(preBranch)
  if (Math.random() < 0.6) {
    step({
      startPint,
      length: preBranch.length,
      angle: preBranch.angle + 0.2,
    })
  }
  if (Math.random() < 0.6) {
    step({
      startPint,
      length: preBranch.length,
      angle: preBranch.angle - 0.2,
    })
  }
}
```

## 递归变堆栈

现在有两个问题，第一，我们做的是生长树，生长树就要有生长的动画，现在是一下就出现的；第二，现在是一个递归，是深度优先的，就是说，其实最先深沉的不是最下面的，反而是最后一个，生长树应该是从上边长出来的

在JavaScript里，可以把一个闭包里的东西先收集起来，不会马上执行，而是把闭包函数，这里的闭包函数是`step()`，把这些函数一个一个放进去。最后一个一个拿出来执行，执行的时候添加一个时间差，最终的效果就是生长树的效果。

```typescript
const pendingTasks: Function[] = []

function step(preBranch: Branch) {
  const startPint: Point = getEndPoint(preBranch)
  drawBranch(preBranch)
  if (Math.random() < 0.6) {
    pendingTasks.push(() => step(...))
  }
  if (Math.random() < 0.6) {
    pendingTasks.push(() => step(...))
  }
}
```

把每一步都先拿到一个数组里，然后再一一释放。

```typescript
function startFrame() {
  requestAnimationFrame(() => {
    frame()
    startFrame()
  })
}
```

`requestAnimationFrame()`与`setInterval()`作用类似，但是不同之处在于，`setInterval()`是按照定死的时间执行的，`requestAnimationFrame()`会根据你的 FPS(Frames Per Second，每秒传输帧数)来，画完上一帧才会画下一帧。
