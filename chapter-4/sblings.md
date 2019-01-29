# 兄弟组件传值

兄弟组件的传值，实际在官方的2.X文档里面没有提及。兄弟组件是结合了父子互相传值的特性，使用一个公共的`EventBus`，兄弟组件传值需要借助一个公共的父组件（Vue实例对象）。

兄弟组件其一将数据传递给父组件，父组件接收这个数据之后再传递给另外一个兄弟组件。

`EventBus`监听兄弟组件之一的事件并接受传值，再下发给另外一个兄弟组件。

首先初始化`EventBus`，实际就是一个Vue对象：

```vue
<!-- eventbus.js 实例化一个 Vue 对象作为 EventBus-->
import Vue from 'Vue'
export default new Vue()
```

这个Vue实例没有DOM,类似一辆Bus穿梭于各个组件之间。

```vue
<!-- 兄弟组件之一  -->
<template>
    <div class="container">
        <h3>我是组件one</h3>
        <span class="one">{{local}}</span>
        <p>点击下面按钮传送数据到组件two</p>
        <button @click="sendFromOneToTwo">传值到组件two</button>
    </div>
</template>
<script>
//子组件引入 eventbus
import EventBus from '@/eventbus'
export default {
    name: 'ComponentOne',
    data() {
        return {
            message: '我来自组件one',
            local: '组件one原始数据'
        }
    },
    methods: {
        sendFromOneToTwo() {
            // 向 eventbus 发送事件
            EventBus.$emit("custom_event_one", this.message);
        }
    },
    mounted: function() {
        //组件One 挂载完之后 监听组件Two 的自定义事件并且对发送来的数据做处理
        EventBus.$on('custom_event_two', (receivedata) => {
            this.local = receivedata
        })
    }
}
</script>
<style>
.container {
    border: 1px solid #ccc;
    text-align: center
}

.one {
    border: 1px solid green;
}
</style>
```

```vue
<!--另外一个兄弟组件 -->
<template>
    <div class="container">
        <h3>我是组件Two</h3>
        <span class="two">{{local}}</span>
        <p>点击下面按钮传送数据到组件one</p>
        <button @click="sendFromTwoToOne">传值到组件one</button>
    </div>
</template>
<script>
//子组件引入 eventbus
import EventBus from '@/eventbus'
export default {
    name: 'ComponentTwo',
    data() {
        return {
            message: '我来自组件two',
            local: '组件two原始数据'
        }
    },
    methods: {
        sendFromTwoToOne() {
            // 向 eventbus 发送事件
            EventBus.$emit("custom_event_two", this.message);
        }
    },
    mounted: function() {
        //组件Two 挂载完之后 监听组件one 的自定义事件并且对发送来的数据做处理
        EventBus.$on('custom_event_one', (receivedata) => {
            this.local = receivedata
        })
    }
}
</script>
<style>
.two {
    border: 1px solid red;
}
</style>
```

```Vue
<!-- 父组件 -->
<template>
    <div id="app">
            <p>{{message}}</p>      	
         <component-one></component-one>
            <hr>
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
    }
}
</script>
```

以上有三个组件，分别是父组件app和子组件one和two，其中one和two属于平级关系。为了让one和two可以通信，再使用了EventBus,在全局注册了一个中间桥梁,并让兄弟组件引用:

```vue
<!-- 注册全局的Bus -->
import Vue from 'Vue'
export default new Vue()
<!-- 需要通信的子组件再引入 -->
import EventBus from '@/eventbus'
```

数据传递需要的中间桥梁已经构建了，然后就是数据传递的方式，首先是数据发送方通过`$emit`方式将需要传输的数据给到bus：

```vue
EventBus.$emit(custom_event,transportData)
```

`custom_event`是一个自定义事件的名称，必须是字符串形式，比如例子中的"custom_event_one"

`transportData` 是要传递出去的数据。

**上面这个“发射自定义事件”需要在组件的`methods`里面定义。**

`$emit`是告诉“中介”，传输一方通过某个自定义的事件要发送数据了，那么接收的一方就需要去监听这个事件并且接收这个数据，于是在另外一个兄弟组件的`methods`：

```vue
EventBus.$on(custom_event,callback)

EventBus.$on(custom_event,(receiveData)=>{ 
    // receiveData接收来自发送方的transportData
    // 并在这里进行处理
})
```

这里的`$on`是负责监听事件的，监听事件的名称和“弹射自定义事件”的名称一致，后面的回调函数用于处理发送来的数据。**这里建议回调函数使用箭头函数的方式，因为只有这样，在回调函数内的this才能指向数据接收方当前组件**。另外，自定义事件的监听一般都是在组件的生命周期钩子函数内。

> 参考 ： https://www.cnblogs.com/tiny-jiao/p/6527449.html
>
> https://juejin.im/post/5b2a05f26fb9a00e70686713
>
> https://www.jianshu.com/p/eeeefde4f3b7

