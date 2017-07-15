#  水平/垂直滑动模型

> A Vue.js project

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```

之前做过移动端垂直滑动效果的页面，整理出来以免忘记了。

之前是用react写，最近研究vue，所以用vue来写，不过基本都是js代码。（transition暂未研究）

scrollDemo.vue文件是滑动组件，通过引入组件，设置isVertical（true为垂直滑动，false为水平滑动），例如：

```javascript
//App.vue
<template>
  <div id="app">
    <scrollDemo :isVertical='false'>
      <div class="container">1<div class="box" @click="dianji"></div></div>
      <div class="container">2</div>
      <div class="container">3</div>
    </scrollDemo>
  </div>
</template>

<script>
// import Hello from './components/Hello'
import scrollDemo from './components/scrollDemo'
export default {
  name: 'app',
  components: {
    scrollDemo
  },
  methods: {
    dianji() {
      alert('可以点击')
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
  width: 100%;
  height: 100%;
}
.box {
  width: 100px;
  height: 120px;
  background-color: red;

}
</style>
>
      
```

由于在微信中，经常会出现touchmove只会触发一次，而且touchend也经常不触发的情况，原因是：主要是由于200ms超时导致内核不一定会一直处理touchmove事件，一旦超时会将后续所有的事件转交给UI处理，导致touchmove不会一直触发。所以在touchstart中添加了event.preventDefault()方法。