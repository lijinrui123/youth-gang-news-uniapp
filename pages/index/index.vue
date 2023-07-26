<template>
	<view class="home">

		<scroll-view scroll-x="true" class="navscroll">
			<view class="item">国内</view>
			<view class="item">国内</view>
			<view class="item">国内</view>
			<view class="item">国内</view>
			<view class="item">国内</view>
			<view class="item">国内</view>
			<view class="item">国内</view>
		</scroll-view>
		<view class="content">
			<view class="row">
				新闻
			</view>
		</view>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: 'Hello'
			}
		},
		onLoad() {

		},
		methods: {

		}
	}
</script>

<style lang="scss" scoped>
	.navscroll {
		height: 100rpx;
		background: #F7F8FA;
		white-space: nowrap;

		// 去除组件自带的滚动条
		/deep/ ::-webkit-scrollbar {
			width: 4px !important;
			height: 1px !important;
			overflow: auto !important;
			background: transparent !important;
			-webkit-appearance: auto !important;
			display: block;
		}

		.item {
			font-size: 40rpx;
			display: inline-block;
			line-height: 100rpx;
			padding: 0 30rpx;
			color: #333;
		}
	}
</style>