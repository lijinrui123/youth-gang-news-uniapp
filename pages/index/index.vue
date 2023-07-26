<template>
	<view class="home">

		<scroll-view scroll-x="true" class="navscroll">
			<view class="item" :class=" index==navIndex ? 'active' :''" v-for="(item,index) in navArr" :key="item.id"
				@click="clickNav(index,item.id)">{{item.classname}}
			</view>
		</scroll-view>
		<view class="content">
			<view class="row" v-for="item in newsArr" :key="item.id">
				<newsbox :item="item" @click.native="goDetail(item)"></newsbox>
			</view>
		</view>
		<view class="nodata" v-if="!newsArr.length">
			<image src="../../static/images/nodata.png" mode=""></image>
		</view>
		<view class="loading" v-if="newsArr.length">
			<view class="">

			</view>
			<view class="" v-if="loading==1">
				数据加载中......
			</view>
			<view class="" v-if="loading==2">
				没有更多了~
			</view>
		</view>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				navIndex: 0,
				navArr: [],
				newsArr: [],
				currentPage: 1,
				loading: 0 //0默认  1 加载中 2没有更多了
			}
		},
		onLoad() {
			this.getNavData();
			this.getNewsData()
		},
		onReachBottom() {
			if (this.loading == 2) {
				return
			}
			this.currentPage++;
			this.loading = 1;
			this.getNewsData()
		},
		methods: {
			// 点击导航切换
			clickNav(index, id) {
				this.navIndex = index;
				// 当前页面重新设为1
				this.currentPage = 1;
				// 将原有的数据清空
				this.newsArr = [];
				this.loading = 0;
				this.getNewsData(id)
			},
			// 跳转到详情页
			goDetail(item) {
				uni.navigateTo({
					url: `/pages/detail/detail?cid=${item.classid}&id=${item.id}`
				})
			},
			// 获取导航列表数据
			getNavData() {
				uni.request({
					url: "https://ku.qingnian8.com/dataApi/news/navlist.php",
					success: (res) => {
						// console.log(res);
						this.navArr = res.data
					}
				})
			},
			// 获取新闻列表数据
			getNewsData(id = 50) {
				uni.request({
					url: "https://ku.qingnian8.com/dataApi/news/newslist.php",
					data: {
						num: 10,
						cid: id,
						page: this.currentPage
					},
					success: (res) => {
						// console.log(res);
						if (res.data.length == 0) {
							this.loading = 2
						}
						this.newsArr = [...this.newsArr, ...res.data]
					}
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.navscroll {
		height: 100rpx;
		background: #F7F8FA;
		white-space: nowrap;
		position: fixed;
		top: var(--window-top);
		left: 0;
		z-index: 10;

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

			&.active {
				color: rgb(25, 152, 50)
			}
		}
	}

	.content {
		padding: 30rpx;
		padding-top: 130rpx;

		.row {
			border: 1px dotted #efefef;
			padding: 20rpx 0;
		}
	}

	.nodata {
		display: flex;
		justify-content: center;

		image {
			width: 360rpx;
		}
	}

	.loading {
		text-align: center;
		font-size: 26rpx;
		color: #888;
		line-height: 2em;

	}
</style>