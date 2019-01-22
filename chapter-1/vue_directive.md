# vue指令

Vue 提供了一些v开头的语法单词，帮助我们使用Vue来操作页面和对应的数据。`v-*`形式就是Vue提供的指令：

[v-text](#v-text)

[v-html](#v-html)

[v-show](#v-show)

[v-if](#单独使用`v-if`)

[v-else](#结合`v-else`)

[v-else-if](#`v-if` `v-else` 和`v-else-if`三个组合一起使用)

[v-for](#v-for)

[v-on](#v-on)

[v-bind](#v-bind)

[v-model](#v-model)

[v-pre](#v-pre)

[v-cloak](#v-cloak)

[v-once](#v-once)

以上[指令](https://cn.vuejs.org/v2/api/#%E6%8C%87%E4%BB%A4)细则可以查询官网API文档，每个指令我们一一分析。指令都是像自定义的元素属性一样，通过指令影响元素：

```html
<div v-show="show" class="info" id="info">
    {{message}}
</div>
```

上面的DIV有自已的ID和class，同时也赋予了一个Vue指令。当然这个DIV一定是在Vue挂载点范围内的。**指令通过后面对它赋予的值来做操作。**

### v-text

`v-text`指令将返回一个字符串，从字面看是文本，所以它更新的是元素的`textContent`，它和元素的`innerText`有些类似，但是有一个巨大的区别：`innerText`展现的就像我们在浏览器看的东西，而且输出形式上没有任何格式（换行，多个空格合并为一），而`textContent`则像去除了全部html标签之后的内容（保留格式信息）。

来一个例子体会一下:

```html
<script src="https://cdn.bootcss.com/vue/2.5.22/vue.js"></script>

<div id="app">
    <p>{{message}}</p>
    <p v-text="message"></p> 
    <p>你好,<p v-text="message"></p></p>  
</div>    

<script>
    var app = new Vue({   
  el: '#app',  
  data: {
    message: 'Hello Vue!'
  }
})   
</script>
```

显示的结果是：

> hello Vue!
>
> hello Vue!
>
> 你好,
>
> hello Vue!

源码:

```html
<p>Hello Vue!</p>
```

替换了

```HTML
<p v-text="message"></p>
```

[在线预览地址](http://jsrun.net/JYXKp/edit)

说明`textContent`保留了`p`标签的换行信息。

### v-html

类似前面的指令，返回的也是一个字符串，但是它是对元素的`innerHTML`有效。`v-html`指令的值将替换使用它的元素。

```html
<script src="https://cdn.bootcss.com/vue/2.5.22/vue.js"></script>

<div id="app">
    <p>使用插值方法:{{message}}</p>
    <p>使用v-html指令方法：<p v-html="message"></p></p> 
</div>    

<script>
    var app = new Vue({   
  el: '#app',  
  data: {
    message: '<span style="color:green;">Hello Vue!</span>'
  }
})   
</script>
```

显示结果:

![v-html指令](img\v-html.png)

源码:

```html
<span style="color:green;">Hello Vue!</span>
```

替换了

```html
<p v-html="message"></p>
```

呈现页面效果。

从效果来说，message值的是字符串，它最终变成HTML被浏览器解析，因而改变了页面DOM结构。官网API文档提示：永远不要在用户提交的内容上使用这个指令，因为容易造成XSS攻击。所以，日常情况还是尽量少使用这个指令。

### v-show

这个指令是用来控制元素的`display`属性的，当这个指令被赋予“真值”的时候，表示元素`display:block`，当是"假值"的时候，元素`display:none`：

```html
<script src="https://cdn.bootcss.com/vue/2.5.22/vue.js"></script>

<div id="app">
    <p v-show="show">我会显示：{{message}}</p>
    <p v-show="hidden">我不显示:{{message}}</p>
</div>    

<script>
    var app = new Vue({   
  el: '#app',  
  data: {
     show:true,
     hidden:false,
     message: 'Hello Vue!'
  }
})   
</script>
```

结果显示：

> 我会显示：Hello Vue!

源码:

```html
<div id="app">
    <p>我会显示：Hello Vue!</p> 
    <p style="display: none;">我不显示:Hello Vue!</p>
</div>
```

也就是

```html
<p>我会显示：Hello Vue!</p> 
```

替换

```html
<!-- v-show 是 true 所以这个元素正常显示-->
<p v-show="true">我会显示：{{message}}</p>
```

而

```html
<p style="display: none;">我不显示:Hello Vue!</p>
```

替换了

```html
<!-- v-show指令是false 所以 元素display:none 也就不显示了 -->
<p v-show="false">我不显示:{{message}}</p>
```

### v-if v-else v-else-if

这三个指令在一起分析，通过名字解读，它们作用于条件判断。类似JS中的`if` 和`else`语句。**需要注意的是只有`v-else`指令后面不要判断条件。**

###### 单独使用`v-if`

```HTML
<script src="https://cdn.bootcss.com/vue/2.5.22/vue.js"></script>

<div id="app">
     <!-- 单独使用 v-if -->
    <p v-if="show">我会显示：{{message}}</p>
    <p v-if="hidden">我不会显示：{{message}}</p>
</div>    

<script>
    var app = new Vue({   
  el: '#app',  
  data: {
     show:true,
     hidden:false,
     message: 'Hello Vue!'
  }
})   
</script>
```

显示结果:

> 我会显示：Hello Vue!

源码：

```html
<div id="app">
    <p>我会显示：Hello Vue!</p> 
    <!---->
</div>
```

发现没有，`v-if`和`v-show`是多么相似，在页面的展现效果上如出一辙，但是，既然是两个指令，肯定有不同的地方。分析解析之后的源码我们可以发现：`v-show`如果取值为“假”，则元素不显示，但是被添加了`display:none`的属性，这个元素依然在DOM里面。而`v-if`如果取值是“假”，则在DOM里面都看不到。上例中只是一个简单的注释。所以，结论显而易见。**`v-if`可以控制元素在DOM中是否创建，而`v-show`只是在CSS层面控制元素是否显示。如果需要频繁切换元素的显示状态，那么使用`v-show`是更好的选择，性能更好。**

###### 结合`v-else`

前面是单独使用`v-if`，这里是结合`v-else`：

```html
<script src="https://cdn.bootcss.com/vue/2.5.22/vue.js"></script>

<div id="app">
    <p>你精通Vue吗？</p>
     <!--  v-if -->
    <span v-if="yes">是的，我很精通它！</span>
     <!-- v-else -->
    <span v-else>没有，我还没入门呢。</span>
</div>    

<script>
    var app = new Vue({   
  el: '#app',  
  data: {
     yes:false,
  }
})   
</script>
```

显示结果：

> 你精通Vue吗？
>
> 没有，我还没入门呢。

源码：

```html
<div id="app">
    <p>你精通Vue吗？</p> 
    <span>没有，我还没入门呢。</span>
</div>
```

前面的代码`v-if`的取值为“假”，所以条件判断进入了`v-else`那一条路。**需要特别注意的一点，`v-else`必须紧紧跟随在`v-if`后面，如果它们被分开了，则会起不到你预想的效果**:

```html
<script src="https://cdn.bootcss.com/vue/2.5.22/vue.js"></script>

<div id="app">
    <p>你精通Vue吗？</p>
     <!--  v-if -->
    <span v-if="yes">是的，我很精通它！</span>
    <p>我是分割线，让v-if和v-else分开了。</p>
     <!-- v-else -->
    <span v-else>没有，我还没入门呢。</span>
</div>    

<script>
    var app = new Vue({   
  el: '#app',  
  data: {
     yes:false,
  }
})   
</script>
```

显示结果：

> 你精通Vue吗？
>
> 我是分割线，让v-if和v-else分开了。

整个判断流程被破坏了，而且控制台也报错:

> v-else used on element &lt;span&gt;without corresponding v-if.

###### `v-if` `v-else` 和`v-else-if`三个组合一起使用

三个指令一起使用，有点类似JS中的switch多条件的判断语句。当需要判断的条件比较多的时候,通常是需要3个和3个以上的不同状态，三个组合比较方便，另外，前面关于`v-if`和`v-else`必须“亲密接触”的原则在这三者之间同样适用:

```html
<script src="https://cdn.bootcss.com/vue/2.5.22/vue.js"></script>

<div id="app">
    <p>今天是周几？</p>
    <span v-if="today==1">今天是周一，上班ing</span>
    <span v-else-if="today==2">今天是周二，上班ing</span>
    <span v-else-if="today==3">今天是周三，上班ing</span>
    <span v-else-if="today==4">今天是周四，上班ing</span>
    <span v-else-if="today==5">今天是周五，上班ing</span>
    <span v-else-if="today==6">今天是周六，还在上班ing</span>
    <span v-else>今天星期天，终于可以休息了</span>
</div>    

<script>
    var app = new Vue({   
  el: '#app',  
  data: {
    today:6
  }
})   
</script>
```

显示结果：

> 今天是周几？
>
> 今天是周六，还在上班ing

**注意，判断表达式中的相等不能使用`=`，这个是赋值符号**

### v-for

`v-for`相当于JS中的`for`循环，多次渲染。基本的语法格式:

```html
<element v-for="item in items">
{{item.properName}}
</element>
```

`element` 是元素标签，比如常用的`div` `li`，`v-for`使用在哪个元素上，这个元素就会多次渲染。  

`item in items` 满足的是特定的`alias in expression`语法，`alias`是当前遍历元素的别名，`expression`是一个复杂的表达式，一般是一个对象或者数组。

如果是数组则在循环的时候除了可以循环输出数组项`item`还可以输出索引`index`：

```html
<ul>
	<li v-for="(item,index) in items">
        {{index}}-{{item}}
	</li>
</ul>
```

`index`取值从0开始。

如果是想循环输出对象`object`的属性`key`和对应的键值`value`：

```html
<ul>
	<li v-for="(value,key,index) in object">
        {{index}}-{{key}}-{{value}}
	</li>
</ul>
```

对象的输出也可以包含索引值,表示键值对的一个序列号：

```js
{name:'chj',sex:'male',age:10}
```

比如这个对象，`index`为2的是“sex:'male'” 这一键值对。

请注意`v-for`循环语法部分的参数声明顺序,针对数组，参数顺序必须是(item,index)，针对对象顺序必须是(value,key,index)，如果有参数不需要则可以忽略。

### v-on

用来监听事件，有点类似jQuery的on方法。它的缩写方式是`@`

```html
<element v-on:eventName="dosomething"></element>
<!-- 缩写 -->
<element @eventName="dosomething"></element>
```

`eventName`是具体的事件名称，比如`click`，`dblclick`等。而`dosomething`则是事件执行的内容，这个内容可以是一个表达式，或者是一个方法，这个方法是`methods`选项里面声明定义的。

```html
<script src="https://cdn.bootcss.com/vue/2.5.22/vue.js"></script>

<div id="app">
    <!-- 使用一个表达式，返回num+1的值 -->
	<a v-on:click="num+=1">加1</a>
    {{num}}
</div>

<script>
    var app=new Vue({
       el:'#app', 	
        data:{
        	num:1
    	}
    })
</script>	
```

```html
<script src="https://cdn.bootcss.com/vue/2.5.22/vue.js"></script>

<div id="app">
    <!-- 使用方法 -->
	<a v-on:click="andOne">加1</a>
    {{num}}
</div>

<script>
    var app=new Vue({
       el:'#app', 	
        data:{
        	num:1
    	},
        // methods 里面定义了andOne方法
        methods:{
            andOne:function(){
                return this.num+=1;
            },
                anotherOne:function(){
                     // dosomething;
                }   
        }
    })
</script>	
```

关于`methods`  后续将再结合`v-on`分析。

### v-bind

`v-bind`是用来绑定属性和属性值的，缩写方式是只要一个冒号`:`。

在日常使用HTML中，元素绑定的属性一般是`class`，`style`，`id`等属性。`v-bind`的存在让我们可以对HTML元素`class`和`style`属性绑定。使用`v-bind`无论是绑定`class`还是`style`都仅限于**对象和数组**语法。

#### 绑定 HTML Class

###### 绑定对象语法

```html
<div :class="{className1:booleanVarValue1,'className2':booleanVarValue2,...}"></div>
```

`className`是CSS Class的名称，`booleanVarValue`是布尔值变量：

```HTML
<style>
    .active{
        border-color:red;
    }
    .text-danger{
        color:yellow;
    }
</style>

<script src="https://cdn.bootcss.com/vue/2.5.22/vue.js"></script>

<div id="app">
      <!-- 绑定的对象 属性名称是 html class 名字部分 -->
      <!-- 属性值 部分是 布尔值变量-->	
	<div v-bind:class="{active:isActive,'text-danger': hasError}">
     </div>
</div>

<script>
    var app=new Vue({
       el:'#app', 	
        data:{
            //数据的键部分是布尔值变量
        	isActive:true,
            hasError:false
    	}
    })
</script>	
```

因为布尔值的运算规则，渲染的HTML：

```html
<div id="app">
	<div class="active">
     </div>
</div>
```

然后再结合CSS声明，最终显示正确的结果，DIV的边框呈现红色。

**绑定HTML CLASS可以以多个键值对形式，然后通过控制变量的"真值"或者“假值”达到是否使用某个Class**

###### 绑定数组语法

```HTML
<div :class="[classVarName1,classVarName2:,...]"></div>
```

`classVarName`变量名，存储CSS Class名 。

```html
<style>
    .active{
        border-color:red;
    }
    .text-danger{
        color:yellow;
    }
</style>

<script src="https://cdn.bootcss.com/vue/2.5.22/vue.js"></script>

<div id="app">
       <!-- 绑定的数组 数组元素是变量 -->
	<div v-bind:class="[isactive,hasError]">
     </div>
</div>

<script>
    var app=new Vue({
       el:'#app', 	
        data:{
            // 键值都是Class 名称
        	isActive:'active',
            hasError:'text-danger'
    	}
    })
</script>	
```

渲染HTML：

```html
<div id="app">
       <!-- 绑定的数组 数组元素是变量 -->
	<div v-bind:class="active text-danger">
     </div>
</div>
```

**绑定CSS Style可以以数组元素形式，每个数组元素用一个变量表示，这个变量的值对应一个CSS Class**

#### 绑定HTML Style 属性

###### 绑定对象语法

这个和绑定`class`有点类似。绑定的语法看起来很像CSS，实际是一个JS对象。**CSS 属性的名称要是么是驼峰要么是短横线分隔符(短横线形式的需要单引号包括)形式**:

```html
<div :style="{styleName1:styleVarValue1,'style_name2':styleVarValue2,...}"></div>
```

`styleName` (驼峰形式)和 `style_name`(短横线形式)都是CSS 属性的名字,`styleValue`则是对应CSS属性的变量。这个变量是在Vue实例的data选项里定义了的。

```html
<script src="https://cdn.bootcss.com/vue/2.5.22/vue.js"></script>

<div id="app">
       <!-- 绑定对象  对象的键都是css属性，对象的键值是一个变量-->
	<div :style="{color:activeColor,fontSize:size+'px','border_color':borderColor}">
     </div>
</div>

<script>
    var app=new Vue({
       el:'#app', 	
        data:{
            //键值都是 stlye 规则值
        	activeColor:'red',
            size:18,
            borderColor:'gray'
    	}
    })
</script>	
```

**这个和绑定HTML Class 类似，对象的属性是`style`的名称，只要控制对象属性值即可。**

对于`style`绑定对象数组的方式，实际绑定到一个对象上更简洁：

```html
<script src="https://cdn.bootcss.com/vue/2.5.22/vue.js"></script>

<div id="app">
       <!-- 绑定对象 直接一个对象搞掂-->
	<div :style="styleCollections">
     </div>
</div>

<script>
    var app=new Vue({
       el:'#app', 	
        data:{
            //键值都是 stlye 规则值
            styleCollections:{
               	activeColor:'red',
            	size:18,
           		borderColor:'gray'      
            }        
    	}
    })
</script>
```

###### 绑定数组语法

可以将多个`style`对象应用到一个元素上：

```html
<div :style="[styleVarName1,'styleVarName2',...]"></div>
```

`styleVarName`是`style`的变量名称，比如"font_size","color"。

```html
<script src="https://cdn.bootcss.com/vue/2.5.22/vue.js"></script>

<div id="app">
	<div :style="[font_color,fontSize,others]">
     </div>
</div>

<script>
    var app=new Vue({
       el:'#app', 	
        data:{
            font_color:{
                color:'red'
            },
            fontSize:{
            	font-size:'24px';
            },
            others:{
                border：'1x solid #ccc',
                padding:0
            }
    	}
    })
</script>	
```

`style`绑定数组，数组元素是一组变量，每个变量可以是多个CSS规则。

#### `v-bind`绑定回顾

`v-bind`可以将`class`属性和CSS类名绑定，也可以将`style`属性和CSS规则绑定，绑定方式可以是对象也可以是数组。

当使用对象方式绑定这两个属性的时候，对象的属性都是对应的类的名称或者CSS规则的名称（不含规则值），需要注意绑定`style`时候，遇到名字复杂的情况下使用驼峰或者下划线形式（下划线名称需要单引号包裹）

当使用数组方式绑定这两个属性的时候，数组元素变量分别对应的是类名和CSS规则（包含规则值）

### v-model

使用场景仅限表单元素 `<input>` `<select>` `<textarea>` 和组件。`v-model`实现了数据双向绑定。

```html
<script src="https://cdn.bootcss.com/vue/2.5.22/vue.js"></script>
<!-- 单行文本 -->
<div id="app">
    <input v-model="message" placeholder="请输入内容..." value="init value">
	<p>Message is: {{ message }}</p>	
</div>

<script>
    var app=new Vue({
       el:'#app', 	
        data:{
            message:'初始化'
        }
    })
</script>
```

上面的代码，虽然给了表单控件一个默认的初始值"init vaule"，但是由于`v-model`的存在，这个初始值会被忽略，数据来源则由Vue实例对象的`data`选项提供。

```html
<script src="https://cdn.bootcss.com/vue/2.5.22/vue.js"></script>
<!-- 多行文本 -->
<div id="app">
	<span>Multiline message is:</span>
	<p style="white-space: pre-line;">{{ message }}</p>
	<br>
	<textarea v-model="message" placeholder="add multiple lines">		</textarea>
</div>

<script>
    var app=new Vue({
       el:'#app', 	
        data:{
            message:'初始化'
        }
    })
</script>
```

```html
<script src="https://cdn.bootcss.com/vue/2.5.22/vue.js"></script>
<!-- 复选框 -->
<div id="app">
	<input type="checkbox" id="checkbox" v-model="checked">
    <label for="checkbox">{{ checked }}</label>
</div>

<script>
    var app=new Vue({
       el:'#app', 	
        data:{
            checked:true
        }
    })
</script>
```

这里的复选框默认是否选中由`data`选项设定。每次点击复选框，随着状态改变，`label`显示的状态也同步改变。

```html
<script src="https://cdn.bootcss.com/vue/2.5.22/vue.js"></script>
<!-- 多个复选框 绑定到一个数组 -->
<div id="app">
	<input type="checkbox" id="checkbox0" value='0' v-model="checkes">
    <label for="checkbox0">0</label>
    <input type="checkbox" id="checkbox1" value='1' v-model="checkes">
    <label for="checkbox1">1</label>
    <input type="checkbox" id="checkbox2" value='2' v-model="checkes">
    <label for="checkbox2">2</label>
    <p>
        选中项都是: {{checkes}}
    </p>
</div>

<script>
    var app=new Vue({
       el:'#app', 	
        data:{
            checkes:[]
        }
    })
</script>
```

```html
<script src="https://cdn.bootcss.com/vue/2.5.22/vue.js"></script>
<!-- 多个复选框 绑定到一个数组 -->
<div id="app">
	<input type="checkbox" id="checkbox0" value='0' v-model="checkes">
    <label for="checkbox0">0</label>
    <input type="checkbox" id="checkbox1" value='1' v-model="checkes">
    <label for="checkbox1">1</label>
    <input type="checkbox" id="checkbox2" value='2' v-model="checkes">
    <label for="checkbox2">2</label>
    <p>
        选中项都是: {{checkes}}
    </p>
</div>

<script>
    var app=new Vue({
       el:'#app', 	
        data:{
            checkes:[]
        }
    })
</script>
```

多个复选框绑定到一个数组，每个复选框都和数组双向绑定。

```html
<script src="https://cdn.bootcss.com/vue/2.5.22/vue.js"></script>
<!-- 单选按钮 -->
<div id="app">
	<input type="radio" id="male" value='男' v-model="sex">
    <label for="male">男</label>
   	<input type="radio" id="female" value='女' v-model="sex">
    <label for="male">女</label>
    <p>
        性别是: {{sex}}
    </p>
</div>

<script>
    var app=new Vue({
       el:'#app', 	
        data:{
           sex:'男'
        }
    })
</script>
```

单选框结果互斥.

```html
<script src="https://cdn.bootcss.com/vue/2.5.22/vue.js"></script>
<!-- 下拉单选按钮 -->
<div id="app">
  <select v-model='selected'>
      <option disabled value="">请选择你的性别：</option>
      <option>男</option>
      <option>女</option>
    </select> 
   <p>你的性别是:{{selected}}</p>   
</div>

<script>
    var app=new Vue({
       el:'#app', 	
        data:{
           selected:''
        }
    })
</script>
```

下拉单选，注意`data`初始化的变量值和`<option disabled>`的值的选择，使用的是空字符串。

```html
<script src="https://cdn.bootcss.com/vue/2.5.22/vue.js"></script>
<!-- 下拉多选按钮 同样是绑定到一个数组-->
<div id="app">
  <select v-model='selected' multiple>
      <option disabled value="">请选择你的性别：</option>
      <option>男</option>
      <option>女</option>
    </select> 
   <p>你的性别是:{{selected}}</p>   
</div>

<script>
    var app=new Vue({
       el:'#app', 	
        data:{
           selected:[]
        }
    })
</script>
```

### v-pre

直接写在元素标签上，不需要表达式，表示使用这个指令的HTML标签不经过Vue的编译。

可以用来显示原始的插值标签，加速编译速度：

```html
<span v-pre>{{ 这里的内容不会编译 }}</span>
```

显示结果：

> \{\{ 这里的内容不会编译 \}\}

　

### v-cloak

也是不需要表达式。使用这个指令的元素，知道编译结束才显示正确的内容，以免当网速过慢显示尴尬的内容。一般和CSS规则一起使用：

```css
[v-cloak]{
    display:none;
}
```

比较好的不显示插值符号：

```html
<div v-cloak>
  {{ message }}
</div>
```

### v-once

同样也是不需要表达式。用在元素或者组件上，只渲染一次。只要使用目的是优化页面刷新性能，如果没有变化的内容可以使用。

```html
<span v-once>这个是不再变化: {{msg}}</span>
```