<template>
  <view class="selector-page">
    <auto-scroll-select
      :value="15"
      :list="selectorOptions"
      keyLabel="name"
      @change="handleAutoScrollSelectChange"
    ></auto-scroll-select>
  </view>
</template>

<script>
import AutoScrollSelect from "../selector/components/AutoScrollSelect/index";
export default {
  components: {
    AutoScrollSelect,
  },
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
      let obj = {
        name: `选项${index0}`,
        value: index0,
      };
      this.selectorOptions.push(obj);
    }
  },
  methods: {
    handleAutoScrollSelectChange(item, index) {
      console.log("AutoScrollSelect change");
      console.log(item);
      console.log(index);
    },
  },
};
</script>

<style scoped lang="scss">
.selector-page {
  padding: 20rpx;
}
</style>
