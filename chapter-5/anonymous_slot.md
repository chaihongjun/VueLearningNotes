# 匿名插槽

匿名插槽其实也是有名字的`default`，但是一般不署名它的`name`属性，所以也就当作匿名处理。

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
    width:3200px;
    padding: 10px 20px;
    background: #f3beb8;
    border: 1px solid #f09898;
}
</style>
```

显示结果：

![匿名插槽](img\anonymous.png)

源码：

```html
<div id="app">
    <p>警告信息!</p>
        <div class="demo-alert-box">
         <strong>Error!</strong>
            404 not found!
    </div>
</div>
```

分析结果发现`<alert-box>404 not found!</alert-box>`被模板解析为了:

```html
<div class="demo-alert-box">
    <strong>Error!</strong>
    404 not found!
</div>
```

再深入，`slot`标签对被`404 not found!`替换，而`404 not found!`则正是父组件的内容。所以，`slot`的位置决定了分发的内容要在哪里显示，而分发显示的内容则是由父组件传递而来，`slot`用来“占位”最合适不过。