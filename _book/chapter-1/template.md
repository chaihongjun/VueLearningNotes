# 模板

前面学习了很多和指令及方法相关的内容。这里学习一下模板概念。Vue中的模板可以简单的理解成使用HTML标签（或则自定义标签，自定义标签后面学习到就是组件名）组合在一起的代码片段。

Vue的模板语法允许以声明方式，将DOM和Vue的实例数据绑定起来。

前面学习的`{{}}`插值语法和`v-*`指令都作用于模板中。

简要内容：

1. 字符串

```vue
Vue.component('my-checkbox', { 
template: `<div class=``"checkbox-wrapper"` `@click=``"check"``><div :class=``"{ checkbox: true, checked: checked }"``></div><div class=``"title"``>{{ title }}</div></div>`,
    ``data() {
        ``return` `{ checked: ``false``, title: ``'Check me'` `}
    ``},
    ``methods: {
        ``check() { ``this``.checked = !``this``.checked; }
    ``}});
```

2. 模板字面量

```vue
Vue.component('my-checkbox', { 
template: `<div class="checkbox-wrapper" @click="check">
                            <div :class="{ checkbox: true, checked: checked }"></div>
                            <div class="title">{{ title }}</div>
                        </div>`,
    data() {
        return { checked: false, title: 'Check me' }
    },
    methods: {
        check() { this.checked = !this.checked; }
    }});
```

3. x-Template

```vue
Vue.component('my-checkbox', { 
   template: '#checkbox-template',
       data() {
           return { checked: false, title: 'Check me' }
       },
       methods: {
           check() { this.checked = !this.checked; }
       }});
```

```html
<script type="text/x-template" id="checkbox-template">
       <div class="checkbox-wrapper" @click="check">
           <div :class="{ checkbox: true, checked: checked }"></div>
           <div class="title">{{ title }}</div>
      </div>
</script>
```

1. 内联模板
2. render方法
3. JSX
4. SFC 单文件组件

常用的还是第1，2，3，7的方法。

关于模板的定义可以参看复习：

> 《7种方法在Vue.js中定义组件模板》](https://chaihongjun.me/javascript/279.html)