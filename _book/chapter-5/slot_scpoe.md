# 作用域插槽

前面的匿名插槽，具名插槽都是不带数据的插槽，直接写在模板内的，而作用域插槽是携带数据的，通过`v-bind`绑定数据。下面的例子除了有传统的使用的具体插槽之外，还引入了两个作用域插槽，由于是多个作用域插槽，所以需要给插槽设置`name`属性，才可以分辨识别。

第一个作用域插槽的数据走向：

1. 父组件通过`:infos="lists" `将父组件的数据`lists`传递给子组件
2. 子组件通过`props:{infos:Array}`接收父组件的值
3. 由于接收的是一个数组，因此在子组件内通过`v-for`将数组进行循环
4. 将需要循环的内容通过`slot`占位，因为循环的内容应该是父组件分发而来，因此需要将数据再次回传给父组件
5. 所以，`<slot :info=info></slot>` 再次数据回传给了父组件。等号后面是子组件循环体内的数据。
6. 父组件再在模板上通过`slot-scope="props"`接收子组件传递而来的数据。这些数据可以被应用到子组件的slot中

```vue
<!-- App.vue -->
<template>
    <div id="app">
        <p>{{message}}</p>
          <!-- 将父组件的数组数据通过 infos 属性传递给子组件 -->
        <alert-box :infos="lists">
            <!-- 具名插槽 -->
            <span slot="default">具名插槽方式：</span>
            <span slot="level">{{level}}</span>
            <span slot="details">{{details}}</span>
            <span slot="zone">插槽作用域方式：</span>
            <!-- 作用域插槽 -->
             <!-- props 是一个自定义的名称 用来接收子组件slot绑定之后传来的数据 -->
              <!-- info 是从子组件传来的数据 -->
            <span slot-scope="props">父组件的值：{{props.info}}</span>
            <!-- todo 是从子组件传来的数据 -->
            <span slot-scope="props" slot="todos">子组件的值：{{props.todo.sex}}-{{props.todo.name}}</span>
        </alert-box>
    </div>
</template>
<script>
import AlertBox from '@/components/AlertBox.vue'
export default {
    data() {
        return {
            message: '警告信息!',
            level: "Waring!",
            details: "404 NOT FOUND!",
            lists: [1, 2, 3, 4, 5]
        }
    },
    components: {
        "alert-box": AlertBox
    }
}
</script>
```

```vue
<template>
    <div class="demo-alert-box">
        <p>
            <slot name="default"></slot>
        </p>
        <p>错误级别：<slot name="level"></slot>
        </p>
        <!-- 这里可以添加其他的信息 -->
        <p>错误详情：<slot name="details"></slot>
        </p>
        <p>
            <slot name="zone"></slot>
        </p>
        <ul>
            <!-- infos 是子组件接收父组件的数据 -->
            <li v-for="info in infos" :key="info.index">
                <!-- 对应第一个作用域插槽 -->
                <!--  通过属性:info 将子组件接受的数据再传回父组件 -->
                <slot :info=info></slot>
            </li>
        </ul>
        <ul>
            <li v-for="todo in todos" :key="todo.index">
                <!-- 对应第二个作用域插槽 -->
                <!--  通过属性:todo 将子组件接受的数据再传回父组件 -->
                <slot :todo=todo name="todos"></slot>
            </li>
        </ul>
    </div>
</template>
<script>
export default {
    data() {
        return {
            msg: "我是子组件的数据",
            todos: [
                { no: 1, sex: 'male', name: 'chj' },
                { no: 2, sex: 'female', name: 'xm' },
                { no: 3, sex: 'female', name: 'xx' }
            ]
        }
    },
    props: {
        infos: Array
    }
}
</script>
<style>
.demo-alert-box {
    width: 300px;
    padding: 10px 20px;
    background: #f3beb8;
    border: 1px solid #f09898;
}
</style>
```

第二个作用域插槽的数据流向解析:

由于示例中有两个作用域插槽，为了分辨出区别，所以给第二个插槽名命`todos`。这个数据走向是直接从子组件向父组件。子组件内`<slot :todo=todo></slot>`通过属性`:todo`将子组件的数据`todo`传递给了父组件。子组件的数据`todo`本身是子组件数据`todos`的子对象。子组件的数据`todo`将会传递到父组件的`slot-scope`属性上，这里还是给这个属性定义名称为`props`。于是，`props.todo`，也就是`todo`这个对象变成了`props`的一个属性。

`slot-scope`的使用场景是经常和需要循环输出的列表或类列表中使用。

>  参考 https://www.jianshu.com/p/a0a83029a217
>
> ​	https://segmentfault.com/a/1190000015884505	
>
> ​	https://www.jianshu.com/p/baf1517e88ef