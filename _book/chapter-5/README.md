# 插槽slot

Vue的`slot`可以类比计算机原件的扩展插槽，计算机的slot用来扩展计算机的能力，比如独立显卡可以放在对应的slot上面提升计算机的显示能力。`slot`常常是和组件组合使用。

```vue
Vue.component('alert-box', {
  template: `
    <div class="demo-alert-box">
      <strong>Error!</strong>
      <slot></slot>
    </div>
  `
})
```

可能渲染的结果是：

> Error！404 Not Found!
>
> 或者
>
> Error ! 403  Forbidden!

渲染的结果需要由`slot`来决定，`slot`决定了分发什么内容。