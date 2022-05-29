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

`onMounted`：Register a callback to be called after the component has been mounted.Param is a callback function.（注册一个回调函数，用以组件安装后调用）

