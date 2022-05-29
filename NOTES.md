## 绑定

```vue
<script setup lang="ts">
const el = ref < HTMLCanvasElement > ()
</script>

<template>
  <div>
    <canvas ref="el" />
  </div>
</template>
```