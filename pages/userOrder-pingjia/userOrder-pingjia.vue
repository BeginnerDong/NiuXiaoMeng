<template>
	<view>
		
		<view class="mglr4">
			<view class="myOrderList fs12">
				<view class="item">
					<view class="flexRowBetween stateLine pdtb15 borderB1">
						<view>订单编号：15558755542525</view>
						<view class="pubColor flexEnd">已提货</view>
					</view>
					<view class="flexRowBetween pdtb10 inforLine borderB1">
						<view class="picBox">
							<image src="../../static/images/evaluation-img.png" mode=""></image>
						</view>
						<view class="rr flexEnd">
							<image class="arrowR" src="../../static/images/about-icon.png" mode=""></image>
						</view>
					</view>
					<view class="flexRowBetween pdtb10">
						<view class="color9 fs13">2020.03.04 14:30:20</view>
						<view class="flexEnd">共1件商品 合计:<span class="fs14">￥56</span></view>
					</view>
				</view>
			</view>
			
			<view class="whiteBj radius10 pdlr4 pdt15 pdb15 mgt15">
				<view class="fs13">
					<textarea value="" placeholder="商品满足您的期望吗？分享你的使用心得" />
				</view>
				<view class="flex">
					<view class="flex onloadbtn">
						<image src="../../static/images/evaluation-img0.png" mode=""></image>
					</view>
				</view>
				<view class="flex pjPic">
					<image src="../../static/images/evaluation-img1.png" mode=""></image>
					<image src="../../static/images/evaluation-img1.png" mode=""></image>
					<image src="../../static/images/evaluation-img1.png" mode=""></image>
				</view>
					
				
				
			</view>
		</view>
		
		<view class="submitbtn" style="margin-top: 120rpx;">
			<button class="btn" type="button" @click="Router.navigateTo({route:{path:'/pages/userOrder/userOrder'}})">发表评论</button>
		</view>
		
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
		}
	};
</script>

<style>
	@import "../../assets/style/myOrder.css";
	page{background: #F5F5F5;padding-bottom: 60rpx;}
	textarea{font-size: 26rpx;width: 100%;height:240rpx;padding: 0;}
	.onloadbtn{width: 120rpx;height: 120rpx;}
	.onloadbtn image{width: 100%;height: 100%;}
	.pjPic image{width: 190rpx;height: 190rpx;margin: 30rpx 30rpx 0 0;border-radius: 10rpx;}
	.pjPic image:nth-of-type(3n){margin-right: 0;}
</style>
