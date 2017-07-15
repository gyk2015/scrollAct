<template>
  <div id="app">
    <scrollDemo :isVertical='false'>
      <div class="container">1<div class="box" @touchend="dianji"></div></div>
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
  height: 1200px;
  background-color: red;

}
</style>
