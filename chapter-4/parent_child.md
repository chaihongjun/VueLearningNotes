# 父子组件传值

除了前面介绍的使用`Vue.component`注册全局组件之外，还可以在Vue实例对象内容使用`components`选项来引入实例对象的子组件。

其实除了像`components` 可以注册子组件外，`directives` `filters` `extends`都可以定义对应的内容，它们都必须在一个实例对象或者组件内使用，并且不属于全局对象。

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