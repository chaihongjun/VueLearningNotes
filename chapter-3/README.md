# 其他API，选项和概念

前面学习了一些概念和API，这一章节将学习遗漏不常用的Vue对象选项和API及相关概念。

### Vue.extend

`Vue.extend`表示构造器，通过搭配的参数构造出一个”子类“。而且它的`data`也必须是函数。这个”子类“再经过实例化，就生成了一个组件。`extend`的参数是一个包含多个实例选项的对象：

```javascript
Vue.extend({
    option1:value1,
    option2:value2,
    ...
})
```

`option` 是参数，可选的是`data`  `template` `methods` `computed` `watch` 等。**`data`必须是函数**

```html
<div id="app"></div>

<script>  
// 创建一个Vue对象构造器
var Constructor = Vue.extend({
    template:`<p>{{firstName}}-{{lastName}} aka {{nickName}}</p>` ,
    data():{
    return {
        firstName:'Chai',
        lastName:'HongJun',
        nickName:'CHJ'
     }
   }
})

// 构造器实例化之后挂载到DOM
new Constructor().$mount('#app')
</script>
```

### Vue.set

添加响应式的数据：

```javascript
Vue.set(target,key,value)
```

`target`是要更改的数据源，对象或者数组

`key` 是要设置的属性，数字类型或者是字符串

`value`是新的值

**使用`Vue.set`可以让修改的数据具有响应式，并且能更新到视图。但是`target`不能是Vue对象或者根数据对象。**

```html
<script src="https://cdn.bootcss.com/vue/2.5.22/vue.js"></script>

<div id="app">
    {{message}},我今年已经{{age}}岁了，所以明年就{{age+1}}岁。<br>
     婚姻状态是：{{otherInfo.marital}}
    <button type="button" @click="change">
         改变状态
    </button>
</div>    

<script>
    var app = new Vue({ 
  el: '#app',   
  data: {
    message: 'Hello!',
    age:99，
      otherInfo:{
        marital:'已婚'                
   }
  },
    methods:{
           change:function(){
         // otherInfo 不是根数据，所以可以配置
         // 第二个参数要么是数字 要么是字符串 不要使用  app.otherInfo.marital
         // 字符串 正好是属性名称 
         // 数字 则是 数组的下标
          Vue.set(app.otherInfo,"marital",'离婚')    
      }                   
    }
})
</script>
```

`Vue.set`可以修改现有或新增属性，而且保持响应式，但是这个属性不能是Vue实例对象，或根数据。

上面例子中的`message`和`age`都属于`data`根级别的数据，无法通过`Vue.set`修改。

### Vue.delete

既然前面的`Vue.set`可以添加或者修改响应式数据，那么自然就有删除响应式数据的方式：

```javascript
Vue.delete(target,key)
```

`target`是要更改的数据源，对象或者数组

`key` 是要设置的属性，数字类型或者是字符串

这个全局的API使用场景应该很少。

### Vue.directive

之前我们学习过一堆的`v-*`指令，那些指令属于Vue内置的。而`Vue.directive`则可以全局注册或者获取我们自定义的指令。

```javascript
Vue.directive(id,[definition])
```

看这个指令定义的模式非常的类似组件的定义方式：

`id`  自定义指令的名称 ，字符串形式

`[definition]` 具体的定义项的内容，函数或者一个对象。如果是对象的话可以选择的选项(钩子函数)有：

​   `bind`  只在元素被第一次绑定到这个指令的时候调用。

​   `inserted`  被绑定的元素在插入到父元素的时候调用。

​   `update` 组件的VNode更新的时候调用。

​   `componentUpdated` 组件的VNode和子VNode全部更新之后调用

​   `unbind` 元素解绑的时候调用。

以上钩子函数的参数如下：

`el`，自定义指令绑定的DOM元素。

`binding`：一个对象，包含属性：

    `name` ： 自定义指令的名称，不包含`v-`前缀

​   `value` ：指令绑定的值，比如：`v-customer="1+1"`,2就是绑定的值。

​   `oldValue`：指令绑定的前一个值，只能在`update`和`componentUpdted`中使用

​   `expression`：字符串形式的指令表达式。`v-customer="1+1"`，"1+1"就是表达式

​   `arg`：传递给指令的参数

​   `modifiers`：包含修饰符的对象，比如：`v-customer.prevent="1+1"`，这个modifiers就是{prevent:true}

`VNode`：Vue生成的虚拟DOM节点

`oldVnode`：上一个虚拟节点，只能在`update`和`componentUpdted`中使用

这里有一个包含全部参数和钩子函数的Demo：

```javascript
Vue.directive('custom',{
    bind:function(el,binding,VNode){
        console.log("自定义指令第一次被绑定到元素上执行bind钩子函数")
        console.log("binding 对象包含{name,value,[oldValue],expression,arg,modifiers})
    },
    inserted:function(el,{name,value,[oldValue],expression,arg,modifiers}，VNode){
     console.log("被绑定的元素插入到父元素的时候调用")
},
    update:function(el,{name,value,oldValue,expression,arg,modifiers}，VNode，oldVnode){
     console.log("组件VNode更新的时候调用")
},
    componentUpdated:function(el,{name,value,oldValue,expression,arg,modifiers}，VNode，oldVnode){
     console.log("组件的VNode和其子VNode更新的时候调用")
}, 
    unbind:function(el,binding,VNode){
        console.log("自定义指令解绑的时候调用")
        console.log("binding 对象包含{name,value,[oldValue],expression,arg,modifiers})
    },
    
})
```

一个例子：

```html
<script src="https://cdn.bootcss.com/vue/2.5.22/vue.js"></script>

<div id="app">
    <div v-custom:foo.a.b="message"></div> 
     <input type="text" v-model="msg"> 
     <p>输入的值:{{msg}}</p>
    <button type="button" @click="change(msg)">根据输入值修改数据</button>
</div> 

<script>
    Vue.directive('custom',{
        //元素 初次绑定 自定义指令
        bind:function(el,binding,vnode){
           console.log("成功生成自定义指令：v-"+binding.name+"，该指令绑定到了元素："+el.nodeName); 
           console.log("该指令被赋予的表达式是："+binding.expression+"，表达式的值是："+binding.value);       
        },
        // 虚拟节点发生了变化：指令表达式值改变
        update:function(el,binding,vnode,oldVnode){
           console.log("指令表达式赋值发生了变化："+"指令表达式："+binding.expression+"表达式的值："+binding.value); 
        }
    })
     
    var app = new Vue({ 
  el: '#app',   
  data: {
    message: 'Hello Vue!',
      msg:undefined
  },
    methods:{
        change:function(msg){
           this.message=msg
        }           
     }     
})
</script>
```

### Vue.filter

注册或者获取全局过滤器：

```javascript
Vue.filter(id,[definiction])
```

`id` 是过过滤器的名称，字符串形式

`[definiction]`  用来处理需要过滤的内容的函数

过滤器可以用在`{{}}`插值和`v-bind`表达式，并且在表达式的尾部通过管道`|`表示使用：

```html
<!-- somefilter 是一个过滤器 -->
/{/{message | somefilter/}/}
<div :id="name | somefilter"></div>
```

```javascript
// 注册过滤器 value是传入的原始数据
Vue.filter('my-filter', function (value) {
  // 返回处理后的值
})

// 获取已注册的过滤器
var myFilter = Vue.filter('my-filter')
```

有一个例子，需要将输入的字符串过滤一遍，只保留字母：

```html
<script src="https://cdn.bootcss.com/vue/2.5.22/vue.js"></script>

<div id="app">
   <input type="text" placeholder="请在这里输入内容..." v-model="message">
    <p>
        这里显示过滤之后的内容：/{/{message | keepfigures /}/}
    </p>
</div>

<script>
    Vue.filter('keepfigures',function(value){
        value = value.toString();
        for(let i=0;i<value.length;i++){
            if(value[i])
        } 
        
    })  
    
 var app = new Vue({ 
  el: '#app',   
  data: {
    message: 'Hello Vue!',
      msg:undefined
  }   
})
</script>
```

