# 模板

前面学习了很多和指令及方法相关的内容。这里学习一下模板概念。Vue中的模板可以简单的理解成使用HTML标签（或则自定义标签，自定义标签后面学习到就是组件名）组合在一起的代码片段。

Vue的模板语法允许以声明方式，将DOM和Vue的实例数据绑定起来。

前面学习的`{{}}`插值语法和`v-*`指令都作用于模板中。

简要内容：

1. 字符串，将HTML模板内容包裹在引号内，以字符串形式表示，HTML模板内容必须连续书写不能换行，比较少用这种。

```vue
Vue.component('my-checkbox', { 
template: '<div class="checkbox-wrapper" @click="check"><div :class="{ checkbox: true, checked: checked }"></div><div class="title">{{ title }}</div></div>',
    data() {
        return{ checked: false, title: 'Check me'}
    },
    methods: {
        check() { this.checked = !this.checked; }
    }});
```

2. 模板字面量，使用`\``（反引号）方式将HTML模板包裹起来，允许换行，一般这种比较常用。

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

3. x-Template 模板内容在`text/x-template`里面以script方式包裹起来的。优势是模板内容和组件分离，便于维护组件的模板内容，但是整体简洁性有一定损失。

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

4. SFC 单文件组件

单文件组件，`.vue`一个单独的一个文件就是一个组件，通过`export default`将本组件导出对外接口，本组件内通过`import`将其他组件引入。

关于模板的定义可以参看复习：

> 《7种方法在Vue.js中定义组件模板》](https://chaihongjun.me/javascript/279.html)