<template>
  <div class="ct">
    <div class="tt" :style="{height: itemSize +'px'}">
      <div v-for="(tt, index) in tts" :key="index">{{tt}}</div>
    </div>
    <div class="VirtualList" :style="{height: divH +'px', width:divW + 17 +'px'}">
      <VirtualList :listData="data" :itemSize="itemSize" />
    </div>
  </div>
</template>
<script>
import VirtualList from "@/components/VirtualList.vue";
let arr = [];
for (let i = 0; i < 100000; i++) {
  arr.push({ id: i });
}
export default {
  name: "LongListScroll",
  data() {
    return {
      data: arr,
      itemSize: 40,
      divH: 600,
      divW: 400,
      tts: ["序号", "名称", "代码", "内容"]
    };
  },
  components: {
    VirtualList
  }
};
</script>
<style lang="scss">
.ct {
  margin: 0 auto;
  width: 400px;
}
.VirtualList {
  display: flex;
  align-items: center;
}
.tt {
  width: 400px;
  display: flex;
  align-items: center;
  color: antiquewhite;
  border-bottom: 1px solid #999;
  background-color: rgb(28, 56, 53);
  & div {
    flex: 1;
    text-align: center;
  }
}
</style>
