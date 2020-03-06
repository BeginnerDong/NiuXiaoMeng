<template>
	<view>
		
		<view class="pdlr4 pdt15 whiteBj">
			<view class="seachBox flexCenter color9 fs12" @click="Router.navigateTo({route:{path:'/pages/seach/seach'}})">
				<image class="icon" src="../../static/images/home-icon.png" mode=""></image>
				<view>搜索</view>
			</view>
		</view>
		
		<view class="indHome flexRowBetween whiteBj pdt15">
			<view class="item"  @click="Router.navigateTo({route:{path:'/pages/productList/productList'}})">
				<image src="../../static/images/home-icon1.png"></image>
				<view class="tit">热门</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/productList/productList'}})">
				<image src="../../static/images/home-icon2.png"></image>
				<view class="tit">蔬菜</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/productList/productList'}})">
				<image src="../../static/images/home-icon3.png"></image>
				<view class="tit">水果</view>
			</view>
			<view class="item"@click="Router.navigateTo({route:{path:'/pages/productList/productList'}})">
				<image src="../../static/images/home-icon4.png"></image>
				<view class="tit">鱼类</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/productList/productList'}})">
				<image src="../../static/images/home-icon5.png"></image>
				<view class="tit">蛋类</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/productList/productList'}})">
				<image src="../../static/images/home-icon6.png"></image>
				<view class="tit">肉类</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/productList/productList'}})">
				<image src="../../static/images/home-icon7.png"></image>
				<view class="tit">坚果</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/productList/productList'}})">
				<image src="../../static/images/home-icon8.png"></image>
				<view class="tit">零食</view>
			</view>
		</view>
		<view class="f5H10"></view>
		
		<view class="RFixIcon" style="bottom: 50%;">
			<image src="../../static/images/home-icon9.png" mode=""></image>
		</view>
		
		<view class="mglr4 proList">
			<view class="item">
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
					<view class="robBtn addCar">加入购物车</view>
				</view>
			</view>
			<view class="item">
				<view >
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
							<view class="btn" @click="counter('-')">-</view>
							<view class="num">{{count}}</view>
							<view class="btn add" @click="counter('+')">+</view>
						</view>
					</view>
				</view>
			</view>
			<view class="item">
				<view>
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
					<view class="robBtn">抢光了</view>
				</view>
			</view>
			<view class="item">
				<view>
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
					<view class="robBtn rob">10点准时开抢</view>
				</view>
			</view>
		</view>
		
		
		<!-- 无数据 -->
		<view class="noDataBox">
			<image src="../../static/images/nodata.png" mode=""></image>
		</view>
		
		
		<!--底部tab键-->
		<view class="navbar">
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar1-a.png" />
				</view>
				<view class="text this-text">首页</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/car/car'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar2.png" />
				</view>
				<view class="text">购物车</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/user/user'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar3.png" />
				</view>
				<view class="text">我的</view>
			</view>
		</view>
		<!--底部tab键 end-->
		
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
				count:1
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			counter(type) {
				const self = this;			
				if (type == '+') {
					self.count++;
				} else {
					if (self.count > 1) {
						self.count--;
					}
				};			
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
