---
# try also 'default' to start simple
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://tse1-mm.cn.bing.net/th/id/R-C.942c9d8f66d878a7c8c02337a0bae7f6?rik=bQysOYF67frfQA&riu=http%3a%2f%2fwww.pptbz.com%2fpptpic%2fUploadFiles_6909%2f201503%2f2015032715445293.jpg&ehk=W9M8MEw5rG9o8uEP7SNJWP8LUp%2f7NT59UaI31hvV3Sw%3d&risl=&pid=ImgRaw&r=0
# apply any windi css classes to the current slide
class: "text-center"
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# show line numbers in code blocks
lineNumbers: false
# some information about the slides, markdown enabled
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# persist drawings in exports and build
drawings:
  persist: false
---

# 长列表性能优化

前端技术分享

<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    开&nbsp;始 <carbon:arrow-right class="inline"/>
  </span>
</div>

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---

# 分页还是无限下拉？

<br>

在正式开始之前，先简单了解一下分页与无限下拉分别适用的场景。

<br>

### 分页

<br>

> 分页技术是指将内容信息划分成独立的页面来显示。如果你滚到一个页面的底部看到一行数字，这些数字就是当前站点或者应用程序里面的分页。

当用户是在结果列表查找特定的信息而不是仅仅浏览信息流时，分页就是好的选择——用户可以知道结果的准确数量，能够决定在哪里停下或者精读哪些结果，政务类网站以分页显示居多。

<br>

[Bing 必应](https://cn.bing.com/search?q=%E5%88%86%E9%A1%B5)

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---

# 分页还是无限下拉？

<br>

在正式开始之前，先简单了解一下分页与无限下拉分别适用的场景。

<br>

### 无限下拉

<br>

> 无限下拉加载技术使用户在大量成块的内容面前一直滚动查看。这种方法是在你向下滚动的时候不断加载新内容。虽然听起来比较诱人，但该技术并不是一个面向任何网站或应用程序的通用方案。

当你使用滚动作为发现数据的主要方法时，它可能使你的用户在网页上停留更长时间并提升用户参与度。在门户网站与社交媒体中，无限下拉被大量使用。相比点击，滚动操作起来也更加容易，对移动设备很友好。

<br>

[今日头条](https://www.toutiao.com/)

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---

# 无限下拉的两种实现方式

### 两种方式的区别

<br>

#### 懒加载

当页面滚动到底部时，进行下一页内容的查询并将结果添加到结果列表中，这就是懒加载。<br>在这种场景下，列表中的 `dom`元素数量是累加的。

<br>

#### 虚拟滚动

虚拟滚动（也叫虚拟列表），尽管在表现形式上与懒加载相似，<br>但列表中展示的 `dom`元素数量实际是固定的。

---
layout: default
---

<div class="my-layout">

# 懒加载的实现

懒加载的触发条件是**页面滚动到底部**，需要用到 DOM 的三个属性值:

| 名称           | 描述                                     |
| -------------- | ---------------------------------------- |
| `scrollTop`    | 滚动条在 Y 轴上的滚动距离                |
| `clientHeight` | 内容可视区域的高度                       |
| `scrollHeight` | 内容可视区域的高度加上溢出（滚动）的距离 |

页面滚动到底部的条件为 `Math.floor(scrollHeight - scrollTop) === clientHeight`。

</div>

<img class="fixed-right" src="https://raw.githubusercontent.com/ivestszheng/images-store/master/img/20220604173029.png">

<style>
.my-layout {
  width: 50%;
  float: left;
}
.fixed-right{
  position: absolute;
  width: 50%;
  right: 0;
  top: 80px;
}
</style>

---
layout: default
---

<div class="my-layout">

# 懒加载的实现

```vue
<template>
  <div class="container" ref="container">
    <div class="content"
     v-for="(item, index) in shownlist"
    :key="index"
    >
      <div style="width: 100%;height: 1rem;">
        {{ item.id }}
      </div>
    </div>
    <div class="loading" v-show="isBusy">
      loading.....
    </div>
  </div>
</template>
```

</div>

<img class="fixed-right" src="https://raw.githubusercontent.com/ivestszheng/images-store/master/img/%E5%8A%A8%E7%94%BB.gif">

<style>
.my-layout {
  width: 50%;
  float: left;
}
.fixed-right{
  position: absolute;
  width: 45%;
  right: 2.5%;
  top: 120px;
}
</style>

---
layout: default
---

<div class="my-layout">

# Vue原生实现

```vue {all|6|8-9|10-16|all}
<script>
mounted() {
    const obj = this.$refs.container;
    const that = this;

    obj.addEventListener('scroll', function () {
      // 向下取整，解决chrome中scrollTop可以为小数的问题
      if (Math.floor(this.scrollHeight - this.scrollTop)
       === this.clientHeight && that.isBusy === false) {
        // isBusy 实现节流
        that.isBusy = true;

        setTimeout(() => {
          ...
          that.isBusy = false;
        }, 1000);
      }
    });
  }
</script>
```

</div>

<img class="fixed-right" src="https://raw.githubusercontent.com/ivestszheng/images-store/master/img/%E5%8A%A8%E7%94%BB.gif">

<style>
.my-layout {
  width: 50%;
  float: left;
}
.fixed-right{
  position: absolute;
  width: 45%;
  right: 2.5%;
  top: 120px;
}
</style>
---
layout: default
---

<div class="my-layout">

# [vue-infinite-scroll](https://element.eleme.cn/#/zh-CN/component/infiniteScroll) 实现

```vue {all|4-6|all}
<template>
  <div
    class="container"
    v-infinite-scroll="loadMore"
    infinite-scroll-disabled="isBusy"
    infinite-scroll-distance="10"
  >
    <div class="content"
    v-for="(item,index) in shownlist"
    :key="index">
      <div style="width: 100%;height: 1rem;">
        {{item.id}}
      </div>
    </div>
    <div class="loading" v-show="isBusy">
      loading.....
    </div>
  </div>
</template>
```

</div>

<img class="fixed-right" src="https://raw.githubusercontent.com/ivestszheng/images-store/master/img/%E5%8A%A8%E7%94%BB.gif">

<style>
.my-layout {
  width: 50%;
  float: left;
}
.fixed-right{
  position: absolute;
  width: 45%;
  right: 2.5%;
  top: 120px;
}
</style>
---
layout: default
---

<div class="my-layout">

# El-Table 实现

```vue
<template>
  <div>
    <el-table
      :data="tableData"
      style="width: 80%; margin: 0 auto"
      max-height="250"
      v-eltable-load="loadMore"
    >
      ...
    </el-table>
  </div>
</template>
```
</div>

<img class="fixed-right" src="https://raw.githubusercontent.com/ivestszheng/images-store/master/img/el-table%E8%87%AA%E5%AE%9A%E4%B9%89%E6%8C%87%E4%BB%A4.gif">

<style>
.my-layout {
  width: 50%;
  float: left;
}
.fixed-right{
  position: absolute;
  width: 45%;
  right: 2.5%;
  top: 120px;
}
</style>
---
layout: default
---

<div class="my-layout">

# El-Table 实现

```js {all|3}
const eltableLoad = {
  bind: (el, binding) => {
    const selectWrap = el.querySelector('.el-table__body-wrapper');

    selectWrap.addEventListener('scroll', function () {
      if (Math.floor(this.scrollHeight - this.scrollTop) <= 
      this.clientHeight) {
        binding.value();
      }
    });
  },
};
```
</div>

<arrow v-click="1" x1="400" y1="260" x2="230" y2="170" color="#564" width="3" arrowSize="1" />

<img class="fixed-right" src="https://raw.githubusercontent.com/ivestszheng/images-store/master/img/el-table%E8%87%AA%E5%AE%9A%E4%B9%89%E6%8C%87%E4%BB%A4.gif">

<style>
.my-layout {
  width: 50%;
  float: left;
}
.fixed-right{
  position: absolute;
  width: 45%;
  right: 2.5%;
  top: 120px;
}
</style>
---
layout: default
---

<div class="my-layout">

# 虚拟滚动原理

可以看到视口高度是固定的，子元素的高度也是固定的，我们可以推算出一个视口最多可以看到多少个元素。只需改变列表中元素的上下空白占位即可实现虚拟滚动的效果。实现的整体思路如下：

1. 计算容器最大容积数量
2. 监听滚动事件动态截取数据
3. 动态设置上下空白占位（核心）
4. 下拉到底部时请求数据
5. 滚动事件节流定时器优化
6. 设置缓冲区优化快速滚动时的白屏问题

</div>

<img class="fixed-right" src="https://raw.githubusercontent.com/ivestszheng/images-store/master/img/20220606163358.png">

<style>
.my-layout {
  width: 50%;
  float: left;
}
.fixed-right{
  position: absolute;
  width: 45%;
  right: 2.5%;
  top: 120px;
}
</style>
---
layout: default
---

<div class="my-layout">

# 计算容器最大容积数量

列表项等高时，容器最大容积数量 = Math.floor(容器的高度 / 列表每项内容的高度) + 2。

> 假如列表的高度并非固定，而是会随着当视口变化。那么当视口改变时（大小改变或翻转），容器最大容积数量也应发生变化。

```vue
<script>
  data() {
    return {
      shownlist: [],
      itemHeight: 80, // 列表每项内容的高度
      maxVolume: 0, //  容器的最大容积
    };
  },
  mounted() {
    // this.maxVolume = Math.floor(this.$refs.container.clientHeight / this.itemHeight) + 2
    this.getMaxVolume();
    // 如果列表的高度并非固定，而是会随着当视口变化，需要增加监听事件
    // window.onresize = () => this.getMaxVolume();
    // window.orientationchange = () => this.getMaxVolume();
  },
</script>
```
</div>

<img class="fixed-right" src="https://raw.githubusercontent.com/ivestszheng/images-store/master/img/image-20220605220258304.png">

<style>
.my-layout {
  width: 50%;
  float: left;
}
.fixed-right{
  position: absolute;
  width: 45%;
  right: 2.5%;
  top: 120px;
}
</style>
---
layout: default
---

<div class="my-layout">

# 监听滚动事件动态截取数据

监听用户滚动事件，根据滚动位置，动态计算当前可视区域起始数据的索引位置 `beginIndex`，再根据最大容积数量 `maxVolume`，计算结束数据的索引位置 `endIndex`，最后根据 `beginIndex`与 `endIndex`截取长列表需宣显示的数据,代码修改后如下:

> 假如列表的高度并非固定，而是会随着当视口变化。那么当视口改变时（大小改变或翻转），容器最大容积数量也应发生变化。

```vue {all|2,4}
<template>
<!-- .passive 会告诉浏览器你不想阻止事件的默认行为,以提高性能 -->
  <div class="container" ref="container" 
   @scroll.passive="handleScroll">
      <div class="content" 
       v-for="(item, index) in shownList"
       :key="index">
         ...
       </div>
  </div>
</template>
```
</div>

<img class="fixed-right" src="https://raw.githubusercontent.com/ivestszheng/images-store/master/img/image-20220605220258304.png">

<style>
.my-layout {
  width: 50%;
  float: left;
}
.fixed-right{
  position: absolute;
  width: 45%;
  right: 2.5%;
  top: 120px;
}
</style>
---
layout: default
---

<div class="my-layout">

# 监听滚动事件动态截取数据

```vue
<script>
  data() {
    return {
      ...
      beginIndex: 0, // 当前滚动的第一个元素索引
    };
  },
  computed: {
    endIndex() {
      let endIndex = this.beginIndex + this.maxVolume;
      if (!this.dataSource[endIndex]) {
        endIndex = this.dataSource.length - 1;
      }
      return endIndex;
    },
    shownList() {
      return this.dataSource.slice(this.beginIndex, this.endIndex + 1);
    },
  },
</script>
```
</div>

<img class="fixed-right" src="https://raw.githubusercontent.com/ivestszheng/images-store/master/img/image-20220605220258304.png">

<style>
.my-layout {
  width: 50%;
  float: left;
}
.fixed-right{
  position: absolute;
  width: 45%;
  right: 2.5%;
  top: 120px;
}
</style>
---
layout: default
---

<div class="my-layout">

# 监听滚动事件动态截取数据

```vue
<script>
  mounted() {
    this.getMaxVolume();
  },
  methods: {
    // 计算容器的最大容积
    getMaxVolume() {
      const { clientHeight } = this.$refs.container
      this.maxVolume = 
      Math.floor(clientHeight / this.itemHeight) + 2;
    },
    // 滚动行为事件,记录滚动的第一个元素索引
    handleScroll() {
      const { scrollTop } = this.$refs.container
      this.beginIndex = Math.floor(scrollTop / this.itemHeight);
    },
  },
</script>
```
</div>

<img class="fixed-right" src="https://raw.githubusercontent.com/ivestszheng/images-store/master/img/image-20220605220258304.png">

<style>
.my-layout {
  width: 50%;
  float: left;
}
.fixed-right{
  position: absolute;
  width: 45%;
  right: 2.5%;
  top: 120px;
}
</style>
---
layout: default
---

<div class="my-layout">

# 监听滚动事件动态截取数据

```vue
<script>
  mounted() {
    this.getMaxVolume();
  },
  methods: {
    // 计算容器的最大容积
    getMaxVolume() {
      const { clientHeight } = this.$refs.container
      this.maxVolume = 
      Math.floor(clientHeight / this.itemHeight) + 2;
    },
    // 滚动行为事件,记录滚动的第一个元素索引
    handleScroll() {
      const { scrollTop } = this.$refs.container
      this.beginIndex = Math.floor(scrollTop / this.itemHeight);
    },
  },
</script>
```
</div>

<img class="fixed-right" src="https://raw.githubusercontent.com/ivestszheng/images-store/master/img/image-20220605220258304.png">

<style>
.my-layout {
  width: 50%;
  float: left;
}
.fixed-right{
  position: absolute;
  width: 45%;
  right: 2.5%;
  top: 120px;
}
</style>
---
layout: default
---

<div class="my-layout">

# 动态设置上下空白占位

根据 `beginIndex`和 `endIndex`，可以动态计算出上下空白高度。

上下空白占位的实现可以有两种思路:一种是通过 `padding`填充，如[tangbc](https://github.com/tangbc)/**[vue-virtual-scroll-list](https://github.com/tangbc/vue-virtual-scroll-list)**;另一种可以 `transform`偏移来实现，如 [Akryum](https://github.com/Akryum)/**[vue-virtual-scroller](https://github.com/Akryum/vue-virtual-scroller)**。这里我采用第一种方案，核心代码如下：

```vue
<script>
  computed: {
    // 计算上下空白占位高度样式
    blankFilledStyle() {
      return {
        paddingTop: `${this.beginIndex * this.itemHeight}px`,
        paddingBottom: `${(this.dataSource.length - this.endIndex - 1) * this.itemHeight}px`,
      };
    },
  },
</script>
```

</div>

<img class="fixed-right" src="https://raw.githubusercontent.com/ivestszheng/images-store/master/img/%E8%99%9A%E6%8B%9F%E6%BB%9A%E5%8A%A8%E7%9A%84%E5%AE%9E%E7%8E%B0.gif">

<style>
.my-layout {
  width: 50%;
  float: left;
}
.fixed-right{
  position: absolute;
  width: 45%;
  right: 2.5%;
  top: 160px;
}
</style>
---
layout: default
---

<div class="my-layout">

# 下拉到底部时请求数据

同懒加载，修改滚动事件，核心代码如下：

```vue
<script>
 handleScroll() {
      this.beginIndex = Math.floor(this.$refs.container.scrollTop / this.itemHeight);
      if (this.beginIndex + this.maxVolume > this.dataSource.length - 1 && !this.isBusy) {
        console.log('滚动到底部了');
        // 追加请求新的数据
        this.isBusy = true;
        // setTimeout 模拟异步,本来想直接在 mockjs 直接返回 promise 的,但是好像不行
        setTimeout(() => {
          this.addItemsToDataSource();
          this.isBusy = false;
        }, 500);
      }
    },
<script>
```

</div>

<img class="fixed-right" src="https://raw.githubusercontent.com/ivestszheng/images-store/master/img/%E8%99%9A%E6%8B%9F%E6%BB%9A%E5%8A%A8%E7%9A%84%E5%AE%9E%E7%8E%B0.gif">

<style>
.my-layout {
  width: 50%;
  float: left;
}
.fixed-right{
  position: absolute;
  width: 45%;
  right: 2.5%;
  top: 160px;
}
</style>
---
layout: default
---

<div class="my-layout">

# 滚动事件节流定时器优化

可以看到滚动事件触发频率非常高。

```vue
<script>
   handleScroll() {
      if (!this.isScrolling) {
        this.isScrolling = true;
        // 时间间隔 30 ms,比较合适，太大会有很明显的白屏
        const scrollerTimer = setTimeout(() => {
          this.isScrolling = false;
          clearTimeout(scrollerTimer);
        }, 30);
        console.log('触发滚动事件');

        this.setDataBeginIndex();
      }
    },
    // 执行数据设置的相关任务，滚动时间的具体行为
    setDataBeginIndex() {
      this.beginIndex = Math.floor(this.$refs.container.scrollTop / this.itemHeight);
      ...
    },
  }
<script>
```

</div>

<img class="fixed-right" src="https://raw.githubusercontent.com/ivestszheng/images-store/master/img/%E6%BB%9A%E5%8A%A8%E4%BA%8B%E4%BB%B6%E8%A7%A6%E5%8F%91%E9%A2%91%E7%8E%87%E9%AB%98.gif">

<style>
.my-layout {
  width: 50%;
  float: left;
}
.fixed-right{
  position: absolute;
  width: 45%;
  right: 2.5%;
  top: 160px;
}
</style>
---
---

# 设置缓冲区

当快速滚动时用户可能会看到白屏，普遍的优化方案是在计算展示列表时，多渲染一屏或多屏的数据。

> 白屏问题无法从根本上解决，因为这与设备的渲染性能有关。一些开发者会限制用户的最大滚动速度以避免这个问题。

```js
   // 列表中要展示的元素集合
    shownList() {
      let beginIndex = 0;
      beginIndex = this.beginIndex <= this.maxVolume ? 0 : this.beginIndex - this.maxVolume;
      // return this.dataSource.slice(this.beginIndex, this.endIndex + 1);
      return this.dataSource.slice(beginIndex, this.endIndex + 1);
    },
    // 计算上下空白占位高度样式
    blankFilledStyle() {
      let beginIndex = 0;
      beginIndex = this.beginIndex <= this.maxVolume ? 0 : this.beginIndex - this.maxVolume;

      return {
        paddingTop: `${beginIndex * this.itemHeight}px`,
        paddingBottom: `${(this.dataSource.length - this.endIndex - 1) * this.itemHeight}px`,
      };
    },
```
---
---

# 路由切换定位列表滚动位置

假设有一个新闻列表，可以点击内容跳转查看详情。如果每次跳转后返回，列表都会回到第一行，那么用户体验就很不好。

为了解决这个问题，需要用到vue的keep-alive，核心代码如下：

```js
data() {
    return {
      scrollTop: 0, // 记录滚动后距离顶部的距离
    };
},
// 被 keep-alive 缓存的组件激活时调用
activated() {
    this.$nextTick(() => {
      this.$refs.container.scrollTop = this.scrollTop;
    });
},
methods:{
    // 执行数据设置的相关任务，滚动事件的具体行为
    setDataBeginIndex() {
      this.scrollTop = this.$refs.container.scrollTop;
      this.beginIndex = Math.floor(this.$refs.container.scrollTop / this.itemHeight);
      ...
    },
}
```
---

# 总结

<br>
<br>

在绝大多数场景，懒加载可以很好地解决客户端与服务端压力，缺点是滚动条是“虚假的”，无法滚动到底部。

虚拟滚动的白屏问题无法从根本上解决，但是真正大量数据渲染场景下，虚拟滚动也许是唯一的解决方案。

<br>
<br>

#### 虚拟滚动拓展：
- el-table 中使用
- 不定高虚拟列表

<br>
<br>

[了解更多](https://ivestszheng.github.io/blog/frontend/vue-infi-scroll-upper.html#%E6%97%A0%E9%99%90%E4%B8%8B%E6%8B%89%E4%B9%8B%E8%99%9A%E6%8B%9F%E6%BB%9A%E5%8A%A8%E7%9A%84%E5%AE%9E%E7%8E%B0)
