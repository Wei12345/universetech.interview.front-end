<template lang="pug">
  .infinite-scroll(:style="rootStyle")
    .infinite-body(:style="bodyStyle")
      .infinite-item(
        v-for="(item, index) in data"
        v-if="isDisplay(index)"
        :key="index"
        :style="itemStyle(index)"
      )
        slot(
          name="item"
          :item="item"
        )
</template>

<script lang="ts">
import { Vue, Component, Prop } from 'vue-property-decorator';

@Component
export default class InfiniteScroll extends Vue {
  @Prop(Number) readonly height!: number;
  @Prop(Number) readonly itemHeight!: number;
  @Prop(Array) readonly data!: any[];

  firstIndex = 0;

  mounted() {
    this.$el.addEventListener('scroll', this.handleScroll);
  }
  beforeDestroy() {
    this.$el.removeEventListener('scroll', this.handleScroll);
  }

  get rootStyle() {
    return {
      height: `${this.height}px`
    };
  }
  get bodyStyle() {
    return {
      height: `${this.itemHeight * this.data.length}px`
    };
  }

  itemStyle(index: number) {
    return {
      height: `${this.itemHeight}px`,
      top: `${this.itemHeight * index}px`
    };
  }

  get displayItemCount(): number {
    return Math.ceil(this.height / this.itemHeight);
  }
  get lastIndex(): number {
    // 顯示數目，再加上加上頭尾各一
    return this.firstIndex + this.displayItemCount + 1;
  }
  isDisplay(index: number) {
    return index >= this.firstIndex && index <= this.lastIndex;
  }
  handleScroll() {
    const { scrollTop } = this.$el;
    this.firstIndex = Math.floor(scrollTop / this.itemHeight);
  }
}
</script>

<style lang="scss" scoped>
.infinite-scroll {
  overflow: scroll;
  .infinite-body {
    position: relative;
    .infinite-item {
      position: absolute;
      width: 100%;
    }
  }
}
</style>
