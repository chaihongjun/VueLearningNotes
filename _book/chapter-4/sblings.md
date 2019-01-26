# 兄弟组件传值

兄弟组件的传值，实际在官方的2.X文档里面没有提及。兄弟组件是结合了父子互相传值的特性，使用一个公共的`EventBus`，兄弟组件传值需要借助一个公共的父组件（Vue实例对象）。

兄弟组件其一将数据传递给父组件，父组件接收这个数据之后再传递给另外一个兄弟组件。

`EventBus`监听兄弟组件之一的事件并接受传值，再下发给另外一个兄弟组件。

首先初始化`EventBus`，实际就是一个Vue对象：

```vue
<!-- eventbus.js 实例化一个 Vue 对象作为 EventBus-->
import Vue from 'Vue'
export default = new Vue()
```

这个Vue实例没有DOM,类似一辆Bus穿梭于各个组件之间。

```vue
<!-- 兄弟组件之一  -->
<template>
	<h3>我是组件one</h3>
	<p>{{local}}</p>
	<p>点击下面按钮传送数据到组件two</p>
  <button @click="sendFromOneToTwo">传值到组件two</button>
</template>
<script>
 //子组件引入 eventbus
import EventBus from './eventbus'
export default {
    name: 'ComponentOne',
    data(){
      return {
          message:'我来自组件one',
          local:'组件one原始数据'
      }
    },
    methods:{
        sendFromOneToTwo(){
            // 向 eventbus 发送事件
           EventBus.$emit("custom_event_one",this.message); 
        }  
    }
}
</script>

```

```vue
<!--另外一个兄弟组件 -->
<template>
  	<h3>我是组件Two</h3>
       <p>{{local}}</p>
	<p>点击下面按钮传送数据到组件one</p>
  <button @click="sendFromTwoToOne">传值到组件one</button>
</template>
<script>
 //子组件引入 eventbus
import EventBus from './eventbus'
export default {
    name: 'ComponentTwo',
    data(){
      return {
            message:'我来自组件two',
            local:'组件two原始数据'
      }
    },
    methods:{
        sendTwo(){
            // 向 eventbus 发送事件
           EventBus.$emit("custom_event_two",this.message); 
        }  
    }
}
</script>
```

```Vue
<!-- 父组件 -->
<template>
    <div id="app">
            <p>{{message}}</p>
        	
         <component-one></component-one>
        
         <component-two></component-two>
        
    </div>
</template>
<script>
import ComponentOne from './components/ComponentOne.vue'
import ComponentTwo from './components/ComponentTwo.vue'
export default {
    name: 'app',
    data(){
      return {
          message:'我是父组件',
      }
    },
    components: {
        ComponentOne,
        ComponentTwo
    },
    methods:{

    }
}
</script>

```

