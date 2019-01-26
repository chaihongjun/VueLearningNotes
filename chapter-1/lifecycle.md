# 生命周期

生命周期，是Vue实例对象的一生。来自官网的图：

![生命周期](img\lifecycle.png)

生命周期有几个关键节点：

`beforeCreate` 表示Vue实例在创建之前，这个时候`el`和`data`都没有初始化，都是`undefined`

`created` 表示Vue实例已经创建，这个时候开始完成了`data`的初始化

`beforeMount` 表示Vue对象在挂载到DOM之前，这个时候`el`也初始化完毕，VDOM也注入完成

`mounted` 表示Vue对象挂载到了DOM上，这个时候VDOM解析完毕，替换成了真实的DOM

`beforeUpdate` 表示组件更新之前

`updated` 表示组件完成更新，当`data`发生变化，会在`beforeUpdate`和`updated`之间过渡。

`beforeDestroy` 表示实例对象销毁之前

`destroyed` 表示Vue实例对象已经销毁了

这里有一篇关于Vue的生命周期的参考：

> [Vue2.0 探索之路——生命周期和钩子函数的一些理解](https://segmentfault.com/a/1190000008010666)

