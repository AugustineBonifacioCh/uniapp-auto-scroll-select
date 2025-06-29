<template>
	<view class="auto-scroll-select_component">
		<view class="selector-container" @click="toggleSelector">
			<view class="selector-display">{{ selectedOption || placeholder }}</view>
			<view class="mask" v-if="showSelector === true" @click.stop="toggleSelector"></view>

			<view class="dropdown-container" v-if="showSelector === true">
				<scroll-view class="options-scroll" scroll-y :style="{ height: scrollHeight }" :scroll-top="scrollTop"
					:scroll-with-animation="false">
					<view v-for="(option, index) in list" :key="index" class="option-item"
						:class="{ 'selected-option': selectedIndex === index }" @click.stop="selectOption(option, index)">
						{{ option[keyLabel] }}
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
			list: {
				type: Array,
				default: () => [],
			},
			// 占位符
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
			// list数组中指定对象的目标属性名（默认 'label' ）
			keyLabel: {
				type: String,
				default: "label",
			},
			// list数组中指定对象的目标属性值（默认 'value' ）
			keyValue: {
				type: String,
				default: "value",
			},
			// 自定义选项的样式
			customStyle: {
				type: [Object, String],
				default: () => {
					return {};
				}
			},
			// 自定义placeholder的样式
			placeholderStyle: {
				type: [Object, String],
				default: () => {
					return {};
				}
			}
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
			// 监听value属性的变化, 当value改变时，更新selectedIndex和selectedOption
			value: {
				handler(newVal) {
					if (newVal !== "") {
						const index = this.list.findIndex(
							(option) => option[this.keyValue] === newVal
						);
						if (index >= 0) {
							this.selectedIndex = index;
							this.selectedOption = this.list[index][this.keyLabel];
							if (this.showSelector) {
								this.$nextTick(() => {
									this.scrollTop = index * 40;
								});
							}
						}
					}
				},
			},
			// 监听list数组的变化，当list数组改变时，更新selectedIndex和selectedOption
			list: {
				immediate: true,
				deep: true,
				handler(newVal) {
					if (newVal.length > 0) {
						const index = this.list.findIndex(
							(option) => option[this.keyValue] === this.value
						);
						if (index >= 0) {
							this.selectedIndex = index;
							this.selectedOption = this.list[index][this.keyLabel];
						}
					}
				}
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
			selectOption(item, index) {
				this.selectedIndex = index;
				this.selectedOption = this.list[index][this.keyLabel];
				this.showSelector = false;
				this.$emit("change", item, index);
			},
		},
	};
</script>

<style scoped lang="scss">
	$select_height: 40px;
	$select_zIndex: 10080;

	.auto-scroll-select_component {
		width: 100%;

		.selector-container {
			position: relative;
			width: 100%;
		}

		.selector-label {
			font-size: 30rpx;
			color: #333;
			margin-bottom: 16rpx;
			display: block;
		}

		.selector-display {
			position: relative;
			display: flex;
			justify-content: center;
			align-items: center;
			height: $select_height;
			line-height: 80rpx;
			background-color: #fff;
			border: 1px solid #ddd;
			box-sizing: border-box;
			padding: 0 16rpx;
			font-size: 15px;
			z-index: $select_zIndex + 1;
		}

		.mask {
			position: fixed;
			top: 0;
			left: 0;
			right: 0;
			bottom: 0;
			background-color: rgba(0, 0, 0, 0.2);
			z-index: $select_zIndex;
		}

		.dropdown-container {
			position: absolute;
			top: $select_height;
			left: 0;
			right: 0;
			background-color: #fff;
			border: 1px solid #ddd;
			box-sizing: border-box;
			border-radius: 8rpx;
			z-index: $select_zIndex;
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