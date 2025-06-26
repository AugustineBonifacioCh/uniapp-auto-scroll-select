<template>
  <view class="selector-page">
    <view class="selector-container" @click="toggleSelector">
      <text class="selector-label">请选择:</text>
      <view class="selector-display">{{ selectedOption || "请选择" }}</view>
      <!-- 灰色遮罩层 -->
      <view
        class="mask"
        v-if="showSelector === true"
        @click.stop="toggleSelector"
      ></view>
      <!-- 下拉选择框 -->
      <view class="dropdown-container" v-if="showSelector === true">
        <scroll-view
          class="options-scroll"
          scroll-y
          :style="{ height: '300px' }"
          :scroll-top="scrollTop"
          :scroll-with-animation="false"
        >
          <view
            v-for="(option, index) in selectorOptions"
            :key="index"
            class="option-item"
            @click.stop="selectOption(index)"
          >
            {{ option }}
          </view>
        </scroll-view>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      selectedIndex: -1,
      selectedOption: "",
      showSelector: false,
      scrollTop: 0,
      selectorOptions: [],
    };
  },
  onLoad(options) {
    for (let index0 = 0; index0 < 30; index0++) {
      this.selectorOptions.push(`选项${index0}`);
    }
  },
  methods: {
    toggleSelector() {
      this.showSelector = !this.showSelector;
      if (this.showSelector && this.selectedIndex >= 0) {
        this.$nextTick(() => {
          this.scrollTop = this.selectedIndex * 40; // 每个选项高度为40px
        });
      }
    },
    selectOption(index) {
      this.selectedIndex = index;
      this.selectedOption = this.selectorOptions[index];
      this.showSelector = false;
    },
  },
};
</script>

<style scoped lang="scss">
.selector-page {
  padding: 20rpx;
}

.selector-container {
  position: relative;
  background-color: #f5f5f5;
  border-radius: 8rpx;
  padding: 16rpx;
}

.selector-label {
  font-size: 30rpx;
  color: #333;
  margin-bottom: 16rpx;
  display: block;
}

.selector-display {
  height: 80rpx;
  line-height: 80rpx;
  background-color: #fff;
  border: 1px solid #ddd;
  border-radius: 8rpx;
  padding: 0 16rpx;
  font-size: 30rpx;
}

.mask {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  // background-color: rgba(0, 0, 0, 0.5);
  background-color: rgba(0, 0, 0, 0.2);
  z-index: 999;
}

.dropdown-container {
  position: absolute;
  top: 100%;
  left: 0;
  right: 0;
  background-color: #fff;
  border: 1px solid #ddd;
  border-radius: 8rpx;
  margin-top: 16rpx;
  z-index: 1000;
}

.options-scroll {
  width: 100%;
}

.option-item {
  // 这里的单位为px
  height: 40px;
  line-height: 40px;
  padding: 0 16rpx;
  border-bottom: 1px solid #eee;
}

.option-item:last-child {
  border-bottom: none;
}

.option-item:active {
  background-color: #f5f5f5;
}
</style>
