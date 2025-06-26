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
          @scroll="scrollHandler"
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
      selectorOptions: [
        "选项1",
        "选项2",
        "选项3",
        "选项4",
        "选项5",
        "选项6",
        "选项7",
        "选项8",
        "选项9",
        "选项10",
      ],
    };
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
  padding: 20px;
}

.selector-container {
  position: relative;
  background-color: #f5f5f5;
  border-radius: 8px;
  padding: 15px;
}

.selector-label {
  font-size: 14px;
  color: #333;
  margin-bottom: 10px;
  display: block;
}

.selector-display {
  width: 100%;
  height: 40px;
  line-height: 40px;
  background-color: #fff;
  border: 1px solid #ddd;
  border-radius: 4px;
  padding: 0 15px;
}

.mask {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.5);
  z-index: 999;
}

.dropdown-container {
  position: absolute;
  top: 100%;
  left: 0;
  right: 0;
  background-color: #fff;
  border: 1px solid #ddd;
  border-radius: 4px;
  margin-top: 5px;
  z-index: 1000;
}

.options-scroll {
  width: 100%;
}

.option-item {
  height: 40px;
  line-height: 40px;
  padding: 0 15px;
  border-bottom: 1px solid #eee;
}

.option-item:last-child {
  border-bottom: none;
}

.option-item:active {
  background-color: #f5f5f5;
}
</style>
