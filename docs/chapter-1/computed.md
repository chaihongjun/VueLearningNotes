# 计算属性

Vue选项，计算属性(`computed`)和后续学习的方法(`methods`)有些类似。计算属性用来完成复杂逻辑内容，将N多计算的东西整合在一个属性内。

```html
<script src="https://cdn.bootcss.com/vue/2.5.22/vue.js"></script>

<div id="app">
    <p>原始内容:{{message}}</p>
    <p>原始内容颠倒之后：{{resverContent}}</p> 
</div>    

<script>
    var app = new Vue({   
  el: '#app',  
  data: {
    message: '今天是星期一！'
  },
  computed:{
      resverContent:function(){
          	return this.message.split('').reverse().join('');
      }        
   } 
})   
</script>
```

显示结果：

> 原始内容:今天是星期一！
>
> 原始内容颠倒之后：！一期星是天今

**`computed`有一个缓存特性，当计算因子不发生改变的时候，`computed`不会再次计算，而是使用已经缓存的计算结果。**

```html
<script src="https://cdn.bootcss.com/vue/2.5.22/vue.js"></script>

<div id="app">
    <input type="text" v-model="message" placeholder="请输入内容...">
    <p>原始内容:{{message}}</p>
    <p>原始内容颠倒之后：{{resverContent}}</p> 
</div>    

<script>
    var app = new Vue({   
  el: '#app',  
  data: {
    message: '今天是星期一！'
  },
  computed:{
      resverContent:function(){
          	return this.message.split('').reverse().join('');
      }        
   } 
})   
</script>
```

显示结果：

![computed计算属性](img\computed1.png)

[在线预览地址：](http://jsrun.top/HiXKp/edit)

在文本框内随意输出修改内容，由于`v-model`双向绑定的缘故，原始内容显示那里会改变，然后造成颠倒算法输出的内容也跟随改变。

再来一个例子：

```html
<script src="https://cdn.bootcss.com/vue/2.5.22/vue.js"></script>

<div id="app">  
    <p>原始内容A={{a}}</p>
    <p>AX2={{getDouble}}</p> 
    <p>A+1={{getPlus}}</p> 
</div> 

<script>
 var vm = new Vue({
    el:'#app', 
  data: { a: 1 },
  computed: {
    // 仅读取
    getDouble: function () {
      return this.a * 2
    },
    // 读取和设置
    getPlus: {
      get: function () {
        return this.a + 1
      },
      set: function (v) {
        this.a = v - 1
      }
    }
  }
})
</script>
```

初始化显示内容：

> 原始内容A=1
>
> AX2=2
>
> A+1=2

这个时候通过`getPlus`的setter，传递一个参数改变一下，在控制台输入：

```javascript
vm.getPlus=1
```

这个时候显示内容变成：

> 原始内容A=0
>
> AX2=0
>
> A+1=1

只要计算属性依赖的因子‘a’发生了变化，计算属性都会重新计算。在实际应用中会遇到页面`reload`等事件，这时候虽然页面重载了，但是依赖的因子没有变化，所以计算属性也不会重新计算。但是由于发生了重新渲染，`methods`方法总是会再次执行。