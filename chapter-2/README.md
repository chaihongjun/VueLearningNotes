# Vue组件初识

前面一个章节学习的都是Vue基础语法部分，但是组件化组件思想是现代前端框架包括Vue在内的一个特点。

关于前端的组件化这里有一篇文章：

> [前端组件化思想](https://www.jianshu.com/p/9445932a1daa)

跟随这篇文章后面的DEMO做一次分析。

首先，进行页面布局分析，将页面分成了【顶部搜索】和【中间内容展示】两个大部分：

![组件大布局](img\components0.png)

而内容部分又可以分为三个部分(part1,part2,part3)：

![内容组件布局](img\components1.png)

每个part部分又可以分为header(title+link)和content：

![内容组件布局](img\components2.png)

从上面的结构可以得出下面的伪代码：

```html
<template>
    <!-- 整个页面容器 -->
  <div id="app">
      <!-- 搜索栏部分 -->
    <div class="nav-search">...</div>
      <!-- 内容部分容器 -->
    <div class="panel">
        <!-- part1 容器 -->
      <div class="part1 left">
          <!-- part1.header 部分 -->
        <div>
          <span>万科城润园楼盘动态</span>
          <a href="">更多动态>></a>
        </div>
           <!-- part1.content 部分 -->
        <div>这里是part1里面的具体内容</div>
      </div>
        <!-- part2 容器 -->
      <div class="part2 right">
          <!-- part2.header 部分 -->
        <div>
          <span>楼盘故事</span>
          <a href="">更多>></a>
        </div>
            <!-- part2.content 部分 -->
        <div>这里是part2里面的具体内容</div>
      </div>
         <!-- part3 容器 -->
      <div class="part3">
          <!-- part3.header 部分 -->
        <div>
          <span>万科城润园户型</span>
          <a href="">二居(1)</a>
          <a href="">三居(4)</a>
          <a href="">四居(3)</a>
          <a href="">更多>></a>
        </div>
            <!-- part3.content 部分 -->
        <div>这里是part3里面的具体内容</div>
      </div>
    </div>
  </div>
</template>

```

从上面代码分析可以看出3个part部分结构都有相似的地方，代码有重复。后期维护将复杂。

这个时候一步步优化上面的代码，将各个part分离，让它们各成为一个独立的文件，于是就有3个独立的文件分离出来了：

```html
<template>
  <div id="app">
    <div class="nav-search">...</div>
    <div class="panel">
      <part1 />
      <part2 />
      <part3 /> 
  </div>
</template>
```

这样基础的文件看起来就简洁了很多，但是各个part文件中的代码还是比较相近适用性差，无法将part文件意志到其他文件中。

于是这里，我们需要提到**组件化**抽象的概念。

三个part都有相同的结构，header和content的组成，以及header的组成，只是各自内容部分不同。

于是我们将这些结构相似的部分抽象出组件来：

```html
<template>
     <!-- part -->
  <div class="part">
      <!-- hearder -->
    <div class="hearder">
        <!-- title -->
      <span>{{ title }}</span>
        <!-- link -->
      <a :href="linkForMore">{{ showMore || '更多>>' }}</a>
    </div>
       <!-- content -->
    <slot name="content" />
  </div>
</template>
```

前面就是组件开发的一个大致的思想，后续详细内容参考前述文章，这里能大致了解一下组件化开发的优势，结构清晰，延展遍历，不会影响其他部分，潜在的未知威胁更小。既然提到了Vue的组件内容，后续将开始展开学习。