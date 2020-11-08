<template>
	<view class="home">
		<swiper indicator-dots circular autoplay>
			<swiper-item v-for="item in swipers" :key="item.id">
				<image :src="item.img"></image>
			</swiper-item>
		</swiper>
		<!-- 导航区域 -->
		<view class="nav">
			<view class="nav_item" v-for="(item,index) in navs" :key="index" @click="navItemClick(item.path)">
				<view :class="item.icon"></view>
				<text>{{item.title}}</text>
			</view>
		</view>
		<!-- 推荐商品区域 -->
		<view class="hot_goods">
			<view class="tit">推荐商品</view>
			<goods-list :goods="goods" @goodsItemClick="goGoodsDetail"></goods-list>
		</view>
	</view>
</template>

<script>
	import goodsList from '../../components/goods-list/goods-list.vue'
	export default {
		data() {
			return {
				swipers: [],
				goods: [],
				navs: [
					{
						icon: 'iconfont icon-chaoshi',
						title: '超级超市',
						path: '/pages/goods/goods'
					},
					{
						icon: 'iconfont icon-guanyuwomen',
						title: '联系我们',
						path: '/pages/contact/contact'
					},
					{
						icon: 'iconfont icon-tupian',
						title: '社区图片',
						path: '/pages/pics/pics'
					},
					{
						icon: 'iconfont icon-shipin',
						title: '学习视频',
						path: '/pages/vide0s/videos'
					}
				]
			}
		},
		onLoad() {
			this.getSwiper()
			this.getHotGoods()
		},
		components: {
			"goods-list": goodsList
		},
		methods: {
			// 获取轮播图数据
			async getSwiper() {
				const res = await this.$myRequest({
					url: '/api/getlunbo'
				})
				this.swipers = res.data.message
			},
			// 获取热门商品数据
			async getHotGoods() {
				const res = await this.$myRequest({
					url:'/api/getgoods?pageindex=1'
				})
				this.goods = res.data.message
			},
			// 导航点击的出来函数
			navItemClick(url) {
				uni.navigateTo({
					url
				})
			},
			// 导航到商品详情页
			goGoodsDetail(id) {
				uni.navigateTo({
					url: '/pages/goods-detail/goods-detail?id=' + id
				})
			}
 		}
	}
</script>

<style lang="scss">
	@import '../../static/font/iconfont.css';
	.home {
		swiper {
			width: 750rpx;
			height: 380rpx;
			image {
				width: 100%;
				height: 100%;
			}
		}
	}
	.nav {
			display: flex;
			.nav_item {
				flex: 1;
				text-align: center;
				view {
					width: 120rpx;
					height: 120rpx;
					border-radius: 50%;
					background: $shop-color;
					margin: 10rpx auto;
					line-height: 120rpx;
					color: #fff;
					font-size: 50rpx;
				}
				.icon-shipin {
					font-size: 40rpx;
				}
				text {
					font-size: 30rpx;
				}
			}
	}
	.hot_goods {
		background: #eee;
		overflow: hidden;
		margin-top: 20rpx;
		.tit {
			height: 100rpx;
			line-height: 100rpx;
			color: $shop-color;
			text-align: center;
			letter-spacing: 40rpx;
			background: #fff;
			margin: 7rpx 0;
		}	
	}
</style>
