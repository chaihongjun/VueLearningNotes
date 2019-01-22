# 事件

`methods`事件处理器，用`v-on`监听的事件都交给这个选项里面定义的事件来处理。

```html
<script src="https://cdn.bootcss.com/vue/2.5.22/vue.js"></script>

<div id="app">
    <p>{{num}}</p>
     <button @click="addOne">单击一次加1</button>
</div>    

<script>
    var app = new Vue({   
  el: '#app',  
  data: {
    num:0
  },
  methods:{
      addOne:function(){
          	return this.num+=1;
      }        
   } 
})   
</script>
```

上面的代码数据初始值是0，随着每点一次按钮，这个值就增加1次。

也可以传参（比较固定）：

```html
<script src="https://cdn.bootcss.com/vue/2.5.22/vue.js"></script>

<div id="app">
    <p>{{num}}</p>
     <button @click="addOne(2)">单击一次加2</button>
</div>    

<script>
    var app = new Vue({   
  el: '#app',  
  data: {
    num:0
  },
  methods:{
      addOne:function(n){
          	return this.num+=n;
      }        
   } 
})   
</script>
```

还可以更灵活：

```html
<script src="https://cdn.bootcss.com/vue/2.5.22/vue.js"></script>

<div id="app">
    <p>{{num}}</p>
     <!-- input 接收值的是字符串类型 后面累加需要转换 -->
    <input type="text" v-model="step">
     <button @click="addOne(step)">单击一次加值</button>
</div>    

<script>
    var app = new Vue({   
  el: '#app',  
  data: {
    num:0,
    step:undefined 
  },
  methods:{
      addOne:function(n){
          	return this.num+=parseInt(n);
      }        
   } 
})   
</script>
```

有时候需要用到DOM事件的事件对象，可以使用`$event`这个特殊变量将事件对象传入方法内:

```html
<script src="https://cdn.bootcss.com/vue/2.5.22/vue.js"></script>

<div id="app">
    <p>{{num}}</p>
     <!-- input 接收值的是字符串类型 后面累加需要转换 -->
    <input type="text" v-model="step">
     <button @click="addOne(step)">单击一次加值</button>
   <button @click="warn('Form cannot be submitted yet.', $event)">
  Submit
</button> 
</div>    

<script>
    var app = new Vue({   
  el: '#app',  
  data: {
    num:0,
    step:undefined 
  },
  methods:{
      addOne:function(n){
          	return this.num+=parseInt(n);
      },
      warn: function (message, event) {
    // 现在我们可以访问原生事件对象
    if (event) event.preventDefault()
    alert(message)
     }
   } 
})   
</script>
```

#### 事件修饰符

对于阻止事件的默认行为(`event.preventDefault()`)和阻止事件冒泡(`event.stopPropagation()`)以及其他的常见需求，Vue 提供了一系列的针对`v-on`指令的修饰符：

###### .stop

作用和`event.stopPropagation`相同，阻止事件冒泡：

```html
<a @click.stop="dosomething"></a>
```

###### .prevent

阻止事件的默认行为，和`event.preventDefault`作用相同：

```HTML
<!-- 提交表单后阻止页面重载（表单提交页面重载是默认行为） -->
<form @submit.prevent="onSubmit"></form>
```

###### .capture

事件监听采用捕获行为，即元素自身出发的事件先处理，然后交给内部元素处理：

```html
<div  @click.capture="dosomething">
    	<a>...</a>
 </div>
```

######　.self

事件监听可以是冒泡或者捕获，它们监听的对象都不一定是触发事件的元素本身。

如果希望`event.target`就是当前元素自身，那么可以使用`.self`修饰符：

```html
<div @click.self="doThat">...</div>
```

当且仅当这个DIV是事件触发源。

当然，**事件修饰符可以串联使用，但必须是合乎逻辑的**：

```html
<a @click.stop.prevent="doThat"></a>
```

阻止事件冒泡和它的默认行为。

`v-on:click.prevent.self` 会先阻止事件冒泡，然后又阻止监听的这个元素，所以会导致，当前元素和内部元素的点击事件都失效。阻止了所有的点击行为。

`v-on:click.self.prevent` 首先点击事件使用的是冒泡行为，然后只有

###### .once

当`v-on`监听的事件只发生一次：

```html
<!-- 点击事件将只会触发一次 -->
<a v-on:click.once="doThis"></a>
```

###### .passive

另外针对`addEventListener`监听器的`passive`参数，Vue也提供了相近的修饰符。

> 关于`passive` https://blog.csdn.net/hhlljj0828/article/details/79497734

有优化移动端，不使用`event.preventDefault()`

#### 按键修饰符

除了事件修饰符,Vue也提供了按键修饰符，可以理解成键盘按键的简略写法。

`.enter`

回车符.

```html
<!-- 按回车的时候 提交-->
<input @keyup.enter="submit">
```

`.tab`

按了键盘的`Tab`键，切换选项卡

`.delete`

键盘的`delete`或者`backspace`键

`.esc`

`.space`

`.up`

`.down`

`.left`

`.right`

另外，可以通过全局的`config.keyCodes`对象定义自己的按键修饰符：

```javascript
<!-- 定义了f1 按键修饰符 -->
Vue.config.keyCodes.f1=112
```

#### 系统修饰键

`.ctrl`

`.alt`

`.shift`

`.meta`

`.exact`  当精确按住某个键的时候才有效