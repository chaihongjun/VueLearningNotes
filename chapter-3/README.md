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

