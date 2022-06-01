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

`onMounted`：Register a callback to be called after the component has been mounted.Param is a callback function.（注册一个回调函数，用以组件安装后调用。参数是回调函数

the `<canvas>` element only has `width` and `height`, both of two attributes.

The id attribute is not personal attribute of the `<canvas>` , it is one of the global HTML attributes and any HTML elements all can apply it.

`getContext()`: this function is used to obtain the rendering context and drawing function of the `<canvas>` element.It takes one parameter, the type of context, like 2d and 3d.这个函数获取`<canvas>`的渲染内容和绘画功能。函数的参数是上下文类型，比如2d和3d。

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
