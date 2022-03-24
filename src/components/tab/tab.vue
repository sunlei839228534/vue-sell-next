<template>
  <div class="tab">
    <cube-tab-bar
      :use-transition="false"
      :show-slider="true"
      v-model="selectedLabel"
      :data="tabs"
      ref="tabBar"
      class="border-bottom-1px"
    ></cube-tab-bar>
    <div class="slide-wrapper">
      <cube-slide
        @change="onChange"
        @scroll="onScroll"
        :options="slideOptions"
        :loop="false"
        :auto-play="false"
        :show-dots="false"
        :initial-index="index"
        ref="slide"
      >
        <cube-slide-item v-for="tab in tabs" :key="tab.label">
          <components :is="tab.component" :data="tab.data"></components>
        </cube-slide-item>
      </cube-slide>
    </div>
  </div>
</template>

<script>
export default {
  name: "tab",
  props: {
    tabs: {
      type: Array,
      default() {
        return [];
      },
    },
    initialIndex: {
      type: Number,
      default: 0,
    },
  },
  data() {
    return {
      index: this.initialIndex,
      slideOptions: {
        listenScroll: true,
        probeType: 3,
        directionLockThreshold: 0,
      },
    };
  },
  methods: {
    onChange(current) {
      this.index = current;
    },
    onScroll(pos) {
      const tabBarWidth = this.$refs.tabBar.$el.clientWidth;
      const slideWidth = this.$refs.slide.slide.scrollerWidth;
      const transform = (-pos.x / slideWidth) * tabBarWidth;
      this.$refs.tabBar.setSliderTransform(transform);
    },
  },
  computed: {
    selectedLabel: {
      get() {
        return this.tabs[this.index].label;
      },
      set(newVal) {
        this.index = this.tabs.findIndex((i) => i.label === newVal);
      },
    },
  },
};
</script>

<style lang="stylus" scoped>
@import "~common/stylus/variable"

.tab
  >>> .cube-tab
    padding: 10px 0
  display: flex
  flex-direction: column
  height: 100%
  .slide-wrapper
    flex: 1
    overflow: hidden
</style>
