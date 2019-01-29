# 匿名插槽

匿名插槽其实也是有名字的`default`，但是一般不署名，所以也就当作匿名处理。

```vue
<!-- App.vue -->
<template>
     <div id="app">
         <p>{{message}}</p>
         <alert-box>404 not found!</alert-box>
     </div>
</template>

<script>
import AlertBox from '@/AlertBox.vue'
  export default {
     data(){
         return {
              message:'警告信息!'
         }
     },
components:{
    "alert-box":AlertBox
  }
}
</script>
```

```vue
<!-- alert-box.vue -->
<template>
    <div class="demo-alert-box">
        <strong>Error!</strong>
        <slot></slot>
    </div>
</template>
<script>
export default {}
</script>
<style>
.demo-alert-box {
    width:200px;
    padding: 10px 20px;
    background: #f3beb8;
    border: 1px solid #f09898;
}
</style>
```

显示结果：

![匿名插槽](img\anonymous.png)