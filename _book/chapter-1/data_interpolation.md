# data选项和插值

前面章节的代码实际已经涉及到了`el` ，`data`和\{\{\}\} 这三个内容。`el`不再赘述，它表示Vue的“势力范围”。这节主要分析`data`选项和\{\{\}\}插值语法的使用。

按照API的介绍，选项`data`主要放置Vue实例对象的变量，从名称也可以得出结论，就是存数据的地方。

而插值语法是将**变量或者表达式**进行解析，得出最终的结果，这里在根据上节的内容，再稍微复杂一些：

```html
<script src="https://cdn.bootcss.com/vue/2.5.22/vue.js"></script>

<div id="app">
    {{message}},我今年已经{{age}}岁了，所以明年就{{age+1}}岁。
</div>    

<script>
    var app = new Vue({ 
  el: '#app',   
  data: {
    message: 'Hello!',
    age:99 
  }
})
</script>
```

显示结果:

> Hello!,我今年已经99岁了，所以明年就100岁。

1. data是一对象，所以有N多个属性。(message,age等等)
2. \{\{age+1\}\} 插值语法中间这个`age+1`就是一个JS表达式
3. **需要注意到的一点，插值语法只能用在HTML的文本内容内，即标签元素内的文本内，HTML标签元素的属性无效。**

```html
<script src="https://cdn.bootcss.com/vue/2.5.22/vue.js"></script>

<div id="app" class="{{font}}">
    {{message}},我今年已经{{age}}岁了，所以明年就{{age+1}}岁。
</div>    

<script>
    var app = new Vue({ 
  el: '#app',   
  data: {
    message: 'Hello!',
    age:99,
    font:'warning'
  }
})
</script>
```

控制台会报错：

>  Interpolation inside attributes has been removed. Use v-bind or the colon shorthand instead. For example, instead of &lt;div class=&quot;\{\{ val \}\}&quot;&gt;, use&lt;div :class=&quot;val&quot;&gt;.

报错信息告诉我们内部属性的插值用法已经被废除了，应该使用`v-bind`替代:

```html
<div class="{{val}}"></div> <!-- 无效 -->
<div :class="val"></div>  <!-- v-bind 替代 -->
```

**只有文本内容可以使用插值方法，元素属性和属性值的绑定必须使用`v-bind`。**至于`v-bind`的用法，后面指令部分将分析。

回到前面插值可以使用JS表达式，JS表达式可以分为简单表达式和复杂表达式(复合表达式)：

1. 简单表达式

   JS内置的关键词(this，null等)，变量，字面量(数字，布尔值，字符串等)这些 ，无法再分解

2. 复杂表达式

   复杂表达式是由简单表达式构成的，对象，数组的初始化表达式，函数定义表达式，属性访问表达式，调用表达式这些都是复杂表达式

插值里面常用的表达式：

1. 对象：\{\{ {age:99 \}\}

2. 字符串： \{\{ 'this is a string!' }\}

3. 布尔值：\{\{ isTrue == 1}\}

4. 三元表达式：\{\{ isTrue?1:0 \}\}

5. 以及上面例子涉及到的带简单运算符的表达式： \{\{ age+1 \}\}

   

   
