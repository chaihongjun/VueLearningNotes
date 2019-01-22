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

`bind`  只在元素被第一次绑定到这个指令的时候调用。

`inserted`  被绑定的元素在插入到父元素的时候调用。

`update` 组件的VNode更新的时候调用。

`componentUpdated` 组件的VNode和子VNode全部更新之后调用

`unbind` 元素解绑的时候调用。