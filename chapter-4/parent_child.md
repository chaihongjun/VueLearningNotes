# 父子组件传值

除了前面介绍的使用`Vue.component`注册全局组件之外，还可以在Vue实例对象内容使用`components`选项来引入实例对象的子组件。

其实除了像`components` 可以注册子组件外，`directives` `filters` `extends`都可以定义对应的内容，它们都必须在一个实例对象或者组件内使用，并且不属于全局对象。另外，Vue实例对象`new Vue()`也可以看作是一个组件。

```html
<script src="https://cdn.bootcss.com/vue/2.5.22/vue.js"></script>

<div id="app">
    <component-a>
        <component-b></component-b>
    </component-a>
</div>

<script> 
// 使用全局注册方式 注册了两个组件
Vue.component('component-a',{
    template:`<p>组件componentA</p>`
})
Vue.component('component-b',{
    template:`<p>组件componentB</p>`
})
    var app=new Vue({
        el:'#app'
    })   
    
</script>    
```

上面使用的是全局的方式注册了两个组件，并且构成了父子结构。但是在实际使用过程中，使用全局方式注册组件不是好的方法，全局组件犹如全局变量，容易因为而非我愿的“出其不意”被修改。

所以，使用局部注册方式更好：

```html
<script src="https://cdn.bootcss.com/vue/2.5.22/vue.js"></script>

<div id="app">
    {{message}}
    <component-a>
        <component-b></component-b>
    </component-a>
</div>

<script> 
    var componentA={
        template:`<p>componentA</p>`,
         // components选项 ，里面注册了A组件的子组件B
            components:{
                  "component-b":componentB
            }
    }
    
    var componentB={
        template:`<p>componentB</p>`,
    }
     
    var app=new Vue({
        el:'#app',
        data:{
            message:'我是根实例！'
        },
        components:{
            "component-a":componentA,
          
        }
    })   
    
</script>   
```

输出结果：

> 我是根实例！
>
> componentA

###### prop 父组件向子组件传值

通过`props`选项，可以实现父向子组件传递数据：

```html
<script src="https://cdn.bootcss.com/vue/2.5.22/vue.js"></script>

<div id="app">
    <span>请输入要传递给子组件的内容：</span> 
    <input type="text" v-model="toSon">
    <son :son-recevier="toSon"></son>
</div>

<script> 
    var son={
        template:`<p>我是子组件son，我接收了来自父组件的内容:{{sonRecevier}},使用本地化数据:{{local}}</p>`,
        props:['sonRecevier']
        // 或者
       // props:{
       //    sonRecevier:String 
        // }
        data:function(){
            return {
                local:this.sonRecevier
            }
        },
            
    }
    
    var app=new Vue({
        el:'#app',
        data:{
            message:'我是根实例！',
            toSon:''
        },
        components:{
            son         
        }
    })       
</script>   
```

`props`可以是一个数组或者是一个对象，需要注意的是它的写法,在HTML中大小写不敏感，所以在组件定义内的写法要么是小驼峰(camelCase)，要么就是短横线(kebab-case)，为了减少不必要的麻麻烦，建议还是短横线写法。

上面的代码作用：当在输入框输入内容的同时，下面会同步显示。

这里`new Vue()`实例化的对象实际也是一个组件，它用`components`选项声明了它的子组件son，从而形式父子组件关系。

父组件的数据`toSon`初始化空字符串，父组件有一个属性`son-recevier`，通过`v-bing`绑定了父组件的数据`toSon`。

再按照语法规则，在子组件定义使用`props`属性表明子组件接受这个父组件属性`son-recevier`，这样父组件的数据就传递给了子组件。

子组件通过属性接收到的数据可以用在子组件内部，比如上例中的模板内。

父传子过程需要注意：

1. 犹如上例中的`toSon`，这里除了可以传递父组件中的变量，也可以直接传递静态的内容，任何类型的值都可以。

2. `props`的接收的属性名称格式必须是`camelCase`或者`kebab-case`，通常建议使用`kebab-case`格式

3. `props`可接受数组和对象，建议使用对象格式，对象格式可以更好的约束对被传递值的一些要求。

   ```javascript
   props:{
       propsA:Number,
       propsB:[Object,Array],
       propsC:{
           type:String,
           required:true,
           default:'100'
       }
   }
   ```

4. `props`接收的父组件的数据会随着父组件的变化而变化，如果接收后不想再受父组件的影响，可以再将接收的数据赋值给子组件`data`，完成本地数据初始化。 

###### 子组件向父组件传值

前面介绍的使用`props`选项方法，是父传子的方向，如果是想将子组件的数据传递给父组件，则需要使用`$emit`方法：**父组件向子组件传递一个Vue自定义事件，子组件通过`$emit`触发这个事件，回调给父组件。**

```vue
<!-- 父组件 -->
<template>
    <div id="app">
        <p>{{message}}</p>
        <!-- customEventName 是Vue自定义的事件名称，将传递给子组件 -->
        <!-- customEventHandler 是对应的事件句柄 这个事件句柄属于父组件 -->
        <Children @custom-event-name="customEventHandler"></Children>
    </div>
</template>
<script>
import Children from './components/Children.vue'
export default {
    name: 'app',
    data(){
      return {
          message:'我是父组件',
      }
    },
    components: {
        Children
    },
    methods:{
        // customEventHandler 父组件事件句柄，将会响应子组件传递而来的数据
       //  dataFromChildren 表示从子组件传递过来的数据
       //  对于这个数据，父组件可以做一些处理
      customEventHandler(dataFromChildren){
         alert(dataFromChildren)
      }
    }
}
</script>

```

```vue
<!-- 子组件 -->
<template>
      <!-- 子组件传递给父组件数据 需要通过 子组件的一个事件 sendToFather  -->
    <button type="button" @click="sendToFather">子组件传父组件值</button>
</template>
<script>
export default {
    name: 'children',
    data() {
        return {
            message: '我是子组件'
        }
    },
    methods: {
        sendToFather() {
            // 在子组件事件内通过$emit通知父组件 去响应自定义事件 customEventName
            // 并且一并带给父组件 数据
            this.$emit("custom-event-name", this.message)
        }
    }
}
</script>
```

通过以上代码的分析来总结一下子组件向父组件传递数据，因为是子传父，所以，我们从子组件开始：

1. 子组件向父组件传值，需要借助子组件的`methods`。

2. 子组件`methods`某个方法将通过`this.$emit(custom-event-name,dataFromChildren)`的方式通知父组件响应自定义事件`custom-event-name`，这个响应是指对这个自定义事件配置对应的事件处理句柄，并且发送子组件数据`dataFromChildren`。

3. 回到父组件视角，由于需要父组件响应前面定义的自定义事件，所以需要在父组件内做监听动作，父组件的方法`customEventHandler`去监听这个事件`custom-event-name`：`@custom-event-name="customEventHandler"`。

4. 然后在父组件的方法`customEventHandler`内，通过参数方式接收来自子组件的数据`dataFromChildren`，最后，可以对这个数据做处理也就是响应自定义事件:

   ```vue
   customEventHandler(dataFromChildren){
            alert(dataFromChildren)
   		// or do other things
         }
   ```

   一句话总结就是，子组件在自己的事件内发起`emit`告诉父组件要监听(`v-on`)一个自定义事件,并且这个自定义事件是可以带数据的，父组件在通过自己的方法监听了这个自定义事件之后，在父组件的方法内对传递来的数据做处理（如果有传递数据的话）。

   **有个注意事项： `custom-event-name`推荐使用下划线命名方式。**

   > 参考 : https://cn.vuejs.org/v2/guide/components-custom-events.html#事件名

