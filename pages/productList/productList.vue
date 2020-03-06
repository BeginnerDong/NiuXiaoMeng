<template>
	<view>
		
		
		<view class="mglr4 proList">
			<view class="item" v-for="(item,index) in mainData" :key="index">
				<view @click="Router.navigateTo({route:{path:'/pages/productDetail/productDetail'}})">
					<view class="center fs13 color6 mgb10">本商品由圣灵商行专供</view>
					<view class="pic">
						<image src="../../static/images/home-banner.png" mode=""></image>
					</view>
					<view class="tit borderB1">智利车厘子 约200g/份 正负20g</view>
					<view class="data flex">
						<view class="mgr20">预售时间：12月12日</view>
						<view>提货时间：12月21日</view>
					</view>
					<view class="sellNum">今日已售<span class="pubcolor">1232</span>份/限量5000份</view>
				</view>
				<view class="flexRowBetween B-price">
					<view class="price fs18 ftw">18.8</view>
					<view class="">
						<view class="numBox flex">
							<view class="btn" @click="counter(index,'-')">-</view>
							<view class="num">{{item.count}}</view>
							<view class="btn add" @click="counter(index,'+')">+</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		
		
		<!-- 无数据 -->
		<view class="noDataBox">
			<image src="../../static/images/nodata.png" mode=""></image>
		</view>
		
		
	</view>
	
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				is_show: false,
				wx_info:{},
				is_show:false,
				mainData:[{count:1},{count:1}]
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			counter(index,type) {
				const self = this;
				
				if (type == '+') {
					self.mainData[index].count++;
				} else {
					if (self.mainData[index].count > 1) {
						self.mainData[index].count--;
					}
				};
				self.$Utils.setStorageArray('cartData', self.mainData[index], 'id', 999);
				
				self.countTotalPrice();
			},
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				self.$apis.orderGet(postData, callback);
			}
		}
	};
</script>

<style>
	@import "../../assets/style/navbar.css";
	@import "../../assets/style/proList.css";
	@import "../../assets/style/numBox.css";
	
	page{padding-bottom: 140rpx;background-color: #F5F5F5;}	
	
	.seachBox{border-radius: 30rpx;background: #f5f5f5;padding:10rpx 80rpx 10rpx 30rpx;position: relative;box-sizing: border-box;}
	.seachBox input{width: 100%;min-height: auto;line-height: 40rpx;height: 40rpx;display: block;font-size: 26rpx;}
	.seachBox .icon{width: 30rpx;height: 30rpx;margin-right: 10rpx;}
	.seachBox .rr{width: 60rpx;height: 40rpx;position: absolute; right: 10rpx;top: 50%;transform: translateY(-50%);}	
	.seachBox .btn{width:100%;height: 100%;background: url(../../static/images/home-icon.png) no-repeat center center /30rpx 30rpx;}
	
	/* 分类导航 */
	.indHome{ flex-wrap:wrap;}
	.indHome .item{width: 25%;text-align: center;padding-bottom: 36rpx; font-size: 26rpx; color: #222;display: flex; flex-direction: column;}
	.indHome .item image{width:90rpx;height:90rpx; margin: 0 auto 16rpx auto; }
	
	
	
</style>
