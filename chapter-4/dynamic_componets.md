# 动态组件

动态组件常常用在组件“形态”相似，但是内容可能不一样的地方,比如多页标签，这里需要用到一个特殊的属性`is`。这个属性后面绑定的是组件的名称：

```vue
<component :is="someComponentName"></component>
```

这里的`someComponentName`是某个组件的名称，一般是一个变量，这个变量接收的组件名称将会决定`<component>`渲染哪个组件。

```vue
<!-- App.vue 父组件 -->
<template>
    <div id="app">
        <button type="button" @click="select('TabOne')">one</button>
        <button type="button" @click="select('TabTwo')">two</button>
        <button type="button" @click="select('TabThree')">three</button>
        <component :is="selected"></component>
    </div>
</template>
<script>
import TabOne from './components/TabOne.vue'
import TabTwo from './components/TabTwo.vue'
import TabThree from './components/TabThree.vue'
export default {
    name: 'app',
    components: {
        TabOne,
        TabTwo,
        TabThree
    },
    data() {
        return {
            selected: null
        }
    },
    methods: {
        select(args) {
            this.selected = args;
        }
    }
}
</script>
<style>
#app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
    width: 300px;
}
</style>
```

```vue
<!-- TabOne.vue -->
<template>
    <div class="tab">
        <p>
            本公司新疆错误排行人物美丽相关看，
            作者控制冲击仔细新浪怎么，
            表明英雄都有创作三大超级现金，
            恐怖士兵多个交换女朋友慢慢心情不
            够请在熟悉很容易顾问网页吉林。</p>
    </div>
</template>
<script>
export default {
    name: 'HelloWorld',
    props: {
        msg: String
    }
}
</script>
```

```vue
<!-- TabTwo.vue -->
<template>
    <div class="tab">
        <p> 许可证焦点用户名笔者毕业哪个最近承诺变化，
            引进看见放弃尚未运输从而出租农业发行设立，
            福建仔细发布时间加工合适最
            近天下地说信誉简介装备本地下载情。</p>
    </div>
</template>
<script>
export default {
    name: 'TabTwo',
    props: {
        msg: String
    }
}
</script>
```

```vue
<!-- TabThree.vue -->
<template>
    <div class="tab">
        <p> 本站证明寂寞看过能够毕业不少二级长大世界杯财富市，
            举行相关图片不敢喜欢列表通用加快统一，规定服装问题不少责任详细隐藏合理工，
            经营很大等待目标删除财富保存熟悉集团很久同志奇迹出售，
            镜头加上同事现代化为此作业看见积分关注但他，
            绝对新闻清除已有门派邮件程序效果活动点评网络老大认证他是，
            工艺不少所谓大多代码住宅课程每，
            审核一脸名称供求可以复杂备案在线大门楼上接近运行环境跟，
            造成破解学生参数怎么会总。</p>
    </div>
</template>
<script>
export default {
    name: 'TabThree',
    props: {
        msg: String
    }
}
</script>
```

每个点击按钮向事件传递的参数都不一样，传入的参数决定了`is`绑定的变量的值。

> 参考： https://blog.csdn.net/m0_37479246/article/details/78915665

