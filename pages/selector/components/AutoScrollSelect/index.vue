<template>
  <view class="auto-scroll-select_component">
    <view class="selector-container" @click="toggleSelector">
      <view class="selector-display">{{ selectedOption || placeholder }}</view>
      <view
        class="mask"
        v-if="showSelector === true"
        @click.stop="toggleSelector"
      ></view>

      <view class="dropdown-container" v-if="showSelector === true">
        <scroll-view
          class="options-scroll"
          scroll-y
          :style="{ height: scrollHeight }"
          :scroll-top="scrollTop"
          :scroll-with-animation="false"
        >
          <view
            v-for="(option, index) in options"
            :key="index"
            class="option-item"
            :class="{ 'selected-option': selectedIndex === index }"
            @click.stop="selectOption(option, index)"
          >
            {{ option.label }}
          </view>
        </scroll-view>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  name: "AutoScrollSelect",
  props: {
    options: {
      type: Array,
      default: () => [],
    },
    placeholder: {
      type: String,
      default: "请选择",
    },
    scrollHeight: {
      type: String,
      default: "300px",
    },
    value: {
      type: [String, Number],
      default: "",
    },
  },
  data() {
    return {
      selectedIndex: -1,
      selectedOption: "",
      showSelector: false,
      scrollTop: 0,
    };
  },
  watch: {
    value: {
      immediate: true,
      handler(newVal) {
        if (newVal !== "") {
          const index = this.options.findIndex(
            (option) => option.value === newVal
          );
          if (index >= 0) {
            this.selectedIndex = index;
            this.selectedOption = this.options[index].label;
            if (this.showSelector) {
              this.$nextTick(() => {
                this.scrollTop = index * 40;
              });
            }
          }
        }
      },
    },
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
    selectOption(item, index) {
      this.selectedIndex = index;
      this.selectedOption = this.options[index].label;
      this.showSelector = false;
      this.$emit("change", item, index);
    },
  },
};
</script>

<style scoped lang="scss">
.auto-scroll-select_component {
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

  .selected-option {
    color: #007aff;
    font-weight: bold;
  }
}
</style>
