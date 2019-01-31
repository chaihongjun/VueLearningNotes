# 作用域插槽

前面的匿名插槽，具名插槽都是不带数据的插槽，直接写在模板内的，而作用域插槽是携带数据的，通过`v-bind`绑定数据。

```vue
<!-- App.vue -->
<template slot-scope="father">
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
        <p>错误级别：<slot name="level" :msg="msg"></slot></p>
         <!-- 这里可以添加其他的信息 -->
        <p>错误详情：<slot name="details" text="来自父组件"></slot></p>
    </div>
</template>
<script>
export default {
    data(){
        return {
            msg:"水平"
        }
    }
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

