# 具名插槽

具名`slot`就是有`name`属性的插槽，这样分发的内容和`slot`可以一一对应，在一个组件内有多个`slot`的时候必然要使用具名插槽。他们的对应关系：

```vue
<slot name="one"></slot>
<template slot="one">/* 这里是模板内容 */</template>
```

`name="one"`指向的插槽和`slot="one"`指向的模板内容匹配对应，也可以不使用`template`直接在元素上：

```vue
<slot name="two"></slot>
<div slot="two">/* 这里是内容 */</div>
```

前面的匿名插槽其实就类似这样：

```vue
<slot name="default"></slot>
<p slot="default">404 not found!</p>
```

匿名插槽的名称是`default`。

```vue
<!-- App.vue -->
<template>
     <div id="app">
         <p>{{message}}</p>
          <alert-box>
              <span slot="level">{{level}}</span>
              <span slot="details">{{details}}</span>
          </alert-box>
     </div>
</template>

<script>
import AlertBox from '@/AlertBox.vue'
  export default {
     data(){
         return {
              message:'警告信息!',
              level:"Waring!",
              details:"404 NOT FOUND!"
         }
     },
components:{
    "alert-box":AlertBox
  }
}
</script>
```

```vue
<template>
    <div class="demo-alert-box">
        <p>错误级别：<slot name="level"></slot></p>
         <!-- 这里可以添加其他的信息 -->
        <p>错误详情：<slot name="details"></slot></p>
    </div>
</template>
<script>
export default {

}
</script>
<style>
.demo-alert-box {
    width:300px;
    padding: 10px 20px;
    background: #f3beb8;
    border: 1px solid #f09898;
}
</style>
```

显示结果：

![具名插槽](img\named_slot.png)

源码：

```html
<div id="app">
		<p>警告信息!</p>
    <div class="demo-alert-box">
           <p>错误级别：<span>Waring!</span></p>
           <p>错误详情：<span>404 NOT FOUND!</span></p>
    </div>
</div>
```

`<slot name="level"></slot>`被替换成了`<span>Waring!</span>`，由父组件内的`<span slot="level">{{level}}</span>`编译而来，`<slot name="details"></slot>`被替换成了`<span>404 NOT FOUND!</span>`，由父组件内的`<span slot="details">{{details}}</span>`编译而来。实现了从父组件向子组件的各个插槽分发不同的内容。