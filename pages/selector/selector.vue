<template>
	<view class="selector-page">
		<k-autoscroll-select :value="currentIndex" :list="selectorOptions" keyLabel="name"
			@change="handleAutoScrollSelectChange"></k-autoscroll-select>
	</view>
</template>

<script>
	export default {
		components: {},
		data() {
			return {
				currentIndex: 0,
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