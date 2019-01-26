# 组件声明

组件化是将页面布局的内容以大化小，以繁化简的一个过程。一个组件的基本定义如下：

```javascript
// 定义一个名为 button-counter 的新组件
Vue.component('button-counter', {
  data: function () {
    return {
      count: 0
    }
  },
  template: '<button @click="count++">You clicked me {{ count }} times.</button>'
})
```

`Vue.component`是全局API，用来注册获取**全局组件**：

```html
Vue.component( id, [definition] )
```

`id`是组件的名称，也就是我们说的HTML标签名称，这不过这里是我们自己定义的。**组件的名称建议使用`PascalCase`大驼峰或`kebab-case`方式。**

`[definition]`一般是一个匿名对象或者函数，不过一般使用的是匿名对象方式：

```javascript
{
   data():{
       msg:'123'
   },
   template:`模板内容`，
   其他选项和选项值
}
```

`template`是DOM选项，会替换挂载的元素。

另外`[definition]`也可以是Vue是的`extend`API扩展的内容：

```javascript
Vue.component( id, Vue.extend({ /*  选项 */ }) )
```

`extend`后续学习分析。

通过最前面的基本DEMO,我们可以看到一个组件由几个重要的选项组成：

组件的名称，这个名称是字符串形式，并且只接受驼峰形式或者短横线形式的命名方式；组件自身的数据`data`(**组件`data`必须是函数类型**，和Vue实例不一样)；组件的字符串模板`template`，里面的内容是由其他的HTML标签组成的代码块。

由于HTML标签不区分大小写，所以无论是使用驼峰还是短横线形式名称的组件名称，最后引用的时候必须都写成小写。

```html
<div id="app">
    <!-- 引用组件 -->
	<button-counter></button-counter>
</div>
```

除了`data`和`template`选项，其他适合Vue实例的选项也适用于组件，因为Vue组件也是一个Vue对象。包括但不限于前面学习的`computed` `methods`另外还有`watch`等选项，当然，只有Vue”根“实例才特有的选项，比如`el`就不适用于组件。

每个组件都是独立的Vue对象，因此，每个组件都有一个自我维护的属于自己的`data`，这样每个组件的状态不会受其他组件干扰。

如果希望一个组件能够影响到另外一个组件，那么就涉及到后续学习的**组件传值**，在学习组件传值之前，先学习一些遗漏的API和Vue对象选项及其其他概念。