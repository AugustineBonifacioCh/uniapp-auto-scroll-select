<template>
  <view class="content">
    <image class="logo" src="/static/logo.png"></image>
    <view class="text-area">
      <text class="title">{{ title }}</text>
    </view>
    <button @click="navigateToSelector" class="nav-button">
      前往选择器页面
    </button>
  </view>
</template>

<script>
export default {
  data() {
    return {
      title: "Hello",
    };
  },
  onLoad() {},
  methods: {
    navigateToSelector() {
      uni.navigateTo({
        url: "/pages/selector/selector",
      });
    },
  },
};
</script>

<style>
.content {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.logo {
  height: 200rpx;
  width: 200rpx;
  margin-top: 200rpx;
  margin-left: auto;
  margin-right: auto;
  margin-bottom: 50rpx;
}

.text-area {
  display: flex;
  justify-content: center;
}

.title {
  font-size: 36rpx;
  color: #8f8f94;
}
.nav-button {
  margin-top: 30rpx;
  width: 200rpx;
  height: 80rpx;
  background-color: #007aff;
  color: white;
  border-radius: 10rpx;
  text-align: center;
  line-height: 80rpx;
  font-size: 32rpx;
}
</style>
