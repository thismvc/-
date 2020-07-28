<template>
  <div ref="list" class="infinite-list-container" :style="{width: containerW + 17 + 'px'}" @scroll="scrollEvent($event)">
    <div class="infinite-list-phantom" :style="{ height: listHeight + 'px',width: '1px' }"></div>
    <div class="infinite-list"  :style="{ transform: getTransform , width: containerW + 'px'}">
      <div
        ref="items"
        class="infinite-list-item"
        :class="activeList === item.id ? 'active' : ''"
        v-for="(item) in visibleData"
        :key="item.id"
        :style="{ height: itemSize + 'px' }"
        @click="handleClick( item )"
      >
        <div>{{ item.id + 1 }}</div>
        <div>名称{{ item.id + 1 }}</div>
        <div>代码{{ item.id + 1 }}</div>
        <div>内容{{ item.id + 1 }}</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "VirtualList",
  props: {
    //所有列表数据
    listData: {
      type: Array,
      default: () => []
    },
    //每项高度
    itemSize: {
      type: Number,
      default: () => 30
    },
    //容器宽度
    containerW: {
        type: Number,
        default: () => 400
    }
  },
  computed: {
    //列表总高度
    listHeight() {
      return this.listData.length * this.itemSize;
    },
    //可显示的列表项数
    visibleCount() {
      return Math.ceil(this.screenHeight / this.itemSize);
    },
    //偏移量对应的style
    getTransform() {
      debugger;
      return `translate3d(0,${this.startOffset}px,0)`;
    },
    //获取真实显示列表数据
    visibleData() {
      return this.listData.slice(
        this.start,
        Math.min(this.end, this.listData.length)
      );
    }
  },
  mounted() {
    this.screenHeight = this.$el.clientHeight;
    this.start = 0;
    this.end = this.start + this.visibleCount;
    console.log("listData", this.listData);
    console.log("listHeight", this.listHeight);
    this.handleKeydown();
  },
  data() {
    return {
      //可视区域高度
      screenHeight: 0,
      //偏移量
      startOffset: 0,
      //起始索引
      start: 0,
      //结束索引
      end: null,
      activeList: 0
    };
  },
  methods: {
    handleKeydown() {
      document.onkeydown = e => {
        //阻止上下方向键会触发滚动事件
        e.preventDefault()
        //主动聚焦
        document.documentElement.focus()
        if (e.keyCode === 38) {//向上
         //找出当前点击的可见列表索引
          let idx = this.visibleData.findIndex(
            item => item.id === this.activeList
          );
          //索引为0向上翻页
          if (idx === 0) {
            this.$refs.list.scrollTo(0,40*(this.start - 15))
            this.start = Math.max(this.start - 1, 0);
            if (this.start != 0) {
              this.end = this.end - 1;
            }
          }
          //显示选中的列表
          this.activeList = Math.max(this.activeList - 1, 0);
        } else if (e.keyCode === 40) {//向下
          let idx = this.visibleData.findIndex(
            item => item.id === this.activeList
          );
          //索引为length-1向下翻页
          if (idx === this.visibleData.length - 1) {
            this.end = Math.min(this.end + 1, this.listData.length);
            this.$refs.list.scrollTo(0,40*(15 + this.start))
            if (this.end != this.listData.length) {
              this.start = this.start + 1;
            }
          }
          //显示选中的列表
          this.activeList = Math.min(
            this.activeList + 1,
            this.listData.length - 1
          );
        } else if (e.keyCode === 13) {//enter
          alert(`您选择了第${this.activeList + 1}行`);
        }
      };
    },
    scrollEvent() {
      //当前滚动位置
      let scrollTop = this.$refs.list.scrollTop;
      //此时的开始索引
      this.start = Math.floor(scrollTop / this.itemSize);
      //此时的结束索引
      this.end = this.start + this.visibleCount;
      //滚动距离不是列表行高的整数倍时多渲染1列
      if (scrollTop % this.itemSize != 0) {
        this.end = this.end + 1;
      }
      //此时的偏移量
      this.startOffset = scrollTop - (scrollTop % this.itemSize);
      console.log(scrollTop, this.start, this.end, this.startOffset);
    },

    handleClick( item ) {
      this.activeList = item.id;
      console.log("this.$refs.list.scrollTop", this.$refs.list.scrollTop);
      alert(`您选择了${this.activeList + 1}行`);
    }
  }
};
</script>


<style lang="scss" scoped>
.infinite-list-container {
  height: 100%;
  overflow-y: auto;
  position: relative;
  -webkit-overflow-scrolling: touch;
  & .infinite-list-phantom {
    position: absolute;
    left: 0;
    top: 0;
    z-index: 1;
  }
  & .infinite-list {
    left: 0;
    top: 0;
    position: absolute;
    text-align: center;
    background-color: rgb(28, 56, 53);

    & .infinite-list-item {
      display: flex;
      align-items: center;
      color: #555;
      box-sizing: border-box;
      border-bottom: 1px solid #999;
      color: antiquewhite;
      cursor: pointer;
      &:hover {
        background-color: rgb(33, 55, 86);
      }
      &.active {
        background-color: rgb(33, 55, 86);
      }
      & div {
        flex: 1;
        text-align: center;
      }
    }
  }
}
</style>