<template>
	<view>
		
		<view class="userHead pdlr4 pubBj white">
			<view class="infor flex pdb20">
				<view>
					<image class="photo" src="../../static/images/about-img.png" mode=""></image>
				</view>
				<view style="width: 70%;">
					<view class="fs16 pdb10 ftw">雨果果</view>
					<view class="fs13">18595682356</view>
				</view>
			</view>
		</view>
		
		<view class="pdlr4">
			<view class="userBox2 boxShaow radius10 whiteBj pdlr4">
				<view class="flexRowBetween pdt15 pdb10 borderB1">
					<view class="ftw">我的订单</view>
					<view class="more flexEnd fs12 color9" @click="Router.navigateTo({route:{path:'/pages/userOrder/userOrder'}})"><image class="arrowR" src="../../static/images/about-icon.png" mode=""></image></view>
				</view>
				<view class="menu flexRowBetween color6 fs12 pdtb15 center">
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/userOrder/userOrder'}})">
						<view class="icon">
							<view class="num">1</view>
							<image src="../../static/images/about-icon1.png"></image>
						</view>
						<view>全部订单</view>
					</view>
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/userOrder/userOrder'}})">
						<view class="icon">
							<view class="num">1</view>
							<image src="../../static/images/about-icon2.png"></image>
						</view>
						<view>未付款</view>
					</view>
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/userOrder/userOrder'}})">
						<view class="icon">
							<view class="num">1</view>
							<image src="../../static/images/about-icon3.png"></image>
						</view>
						<view>待提货</view>
					</view>
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/userOrder/userOrder'}})">
						<view class="icon">
							<view class="num">1</view>
							<image src="../../static/images/about-icon4.png"></image>
						</view>
						<view>已提货</view>
					</view>
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/userOrder/userOrder'}})" >
						<view class="icon">
							<view class="num">1</view>
							<image src="../../static/images/about-icon5.png"></image>
						</view>
						<view>已评价</view>
					</view>
				</view>
			</view>
			
			<view class="boxShaow radius10 whiteBj mgt15 pdlr4 pdb15">
				<view class="flexRowBetween pdt15 pdb10 borderB1" @click="Router.navigateTo({route:{path:'/pages/zitiAddress/zitiAddress'}})">
					<view class="ftw">我的自提点</view>
					<view class="flexEnd fs12 color9" >切换自提点<image class="arrowR" src="../../static/images/about-icon.png" mode=""></image></view>
				</view>
				<view class="pubColor pdt15">门店名称门店名称门店名称</view>
				<view class="fs13 color6 pdt5">地址：陕西省西安市高新区大都荟A座</view>
			</view>
			
			<view class="boxShaow radius10 whiteBj mgt15 pdlr4">
				<view class="flexRowBetween pdtb15" @click="Router.navigateTo({route:{path:'/pages/login/login'}})">
					<view class="ftw">商家入口</view>
					<view class="flexEnd fs12 color9"><image class="arrowR" src="../../static/images/about-icon.png" mode=""></image></view>
				</view>
			</view>
			
			<view class="mgt20 color9 fs12">1.如果您购买的商品有任何问题，请直接与购买的门店联系！100%售后保障！</view>
			<view class="mgt10 color9 fs12">2.如果您购买的商品有任何问题，请直接与购买的门店联系！100%售后保障！</view>
			
			
		</view>
			
		<!--底部tab键-->
		<view class="navbar">
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar1.png" />
				</view>
				<view class="text">首页</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/car/car'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar2.png" />
				</view>
				<view class="text">购物车</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/user/user'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar3-a.png" />
				</view>
				<view class="text this-text">我的</view>
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
				showView: false,
				score:'',
				wx_info:{}
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {


		},
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	@import "../../assets/style/navbar.css";
	page{padding-bottom: 140rpx;background-color: #F5F5F5;}
	.userHead{box-sizing: border-box;padding: 50rpx 4%;height: 306rpx;}
	.userHead .photo{width: 120rpx;height: 120rpx;border-radius: 50%;margin-right: 20rpx;box-sizing: border-box;}
	.headNum .item{width: 50%;font-size: 26rpx;color: #eee;}
	.headNum .item .mny{font-size: 40rpx;line-height: 44rpx;margin-bottom: 14rpx;color: #fff;}
	
	.userBox2{margin-top: -90rpx;position: relative; z-index: 1; box-shadow: 0 10rpx 20rpx rgba(0,0,0,0.1);}
	.userBox2 .item{width: 20%;}
	.userBox2 .item .icon{width: 48rpx;height:48rpx;display: block;margin: 0 auto;margin-bottom: 10rpx;position: relative;}
	.userBox2 .item .icon image{width: 100%;height: 100%;}
	.userBox2 .item .icon .num{line-height: 30rpx;height: 30rpx;min-width: 30rpx;border-radius: 18rpx;text-align: center;color: #fff;background-color: #E1211C;font-size: 18rpx;position: absolute;top: -15rpx;right: -15rpx;z-index: 1;box-sizing: border-box;padding: 0 6rpx;}
	
	
	.myRowBetween .ll .icon{width: 32rpx;height: 32rpx;margin-right: 20rpx;}
</style>
