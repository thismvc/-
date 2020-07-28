<template>
  <div id="app">
    <!-- <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </div>
    <router-view />-->
    <div class="div-tt">长列表渲染</div>
    <router-view />
    <!-- <div class="ct">
      <div class="tt" :style="{height: itemSize +'px'}">
        <div v-for="(tt, index) in tts"  :key="index">{{tt}}</div>
      </div>
      <div class="VirtualList" :style="{height: divH +'px', width:divW + 17 +'px'}">
        <VirtualList :listData="data" :itemSize="itemSize" />
      </div>
    </div> -->
  </div>
</template>
<script>
export default {
  name: "app",
  data() {
    return {
    };
  }
  
};
</script>
<style lang="scss">
#app {
  height: 100%;
  width: 100%;
  .div-tt {
    width: 100%;
    text-align: center;
    color: #2c3e50;
    line-height: 50px;
    font-weight: 600;
    font-size: 18px;
  }
}
* {
  box-sizing: border-box;
}
// #app {
//   font-family: Avenir, Helvetica, Arial, sans-serif;
//   -webkit-font-smoothing: antialiased;
//   -moz-osx-font-smoothing: grayscale;
//   text-align: center;
//   color: #2c3e50;
// }

// #nav {
//   padding: 30px;

//   a {
//     font-weight: bold;
//     color: #2c3e50;

//     &.router-link-exact-active {
//       color: #42b983;
//     }
//   }
// }
</style>
