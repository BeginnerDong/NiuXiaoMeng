<template>
	<view>
		<view class="mglr4">
			<view class="center process flexRowBetween fs13 color6">
				<view class="item on">
					<view class="num"><view>1</view></view>
					<view>提交订单</view>
				</view>
				<view class="item">
					<view class="num"><view>2</view></view>
					<view>付款</view>
				</view>
			</view>
			<view class="whiteBj radius10 center pdlr4 pdt20 pdb25 fs13">
				<view class="">订单金额</view>
				<view class="fs22 mgt10 mgb15 pubColor">￥{{mainData.price?mainData.price:''}}</view>
				<view>订单提交成功，请在10分钟内完成支付</view>
				<view class="mgt15 flexCenter">支付方式：<image style="width: 30rpx;height: 28rpx;margin-right: 6rpx;" src="../../static/images/pay-icon.png" mode=""></image>微信支付</view>
			</view>
			
			<view class="submitbtn" style="margin-top: 200rpx;">
				<button class="btn" type="primary" @click="Utils.stopMultiClick(goPay)">去付款</button>
			</view>
			<!-- 
			<view class="submitbtn" style="margin-top: 200rpx;">
				<button class="btn" type="primary" @click="Router.navigateTo({route:{path:'/pages/orderPayOk/orderPayOk'}})">去付款</button>
			</view> -->
		</view>
		
		<view class="black-bj" v-show="is_show"></view>
		<view class="tips-Show center radius10 whiteBj" v-show="is_tips">
			<view class="fs14 ftw pdt20 pdb15">温馨提示</view>
			<view class="text color6 mgb20 fs13">未获取到支付成功信息，请在10分钟内到订单中继续支付，逾期订单将自动关闭！</view>
			<view class="btn pdtb20 pubColor fs15" @click="self.Router.redirectTo({route:{path:'/pages/userOrder/userOrder'}})">确定</view>
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
				wx_info:{},
				is_show:false,
				is_tips:false,
				mainData:{},
				Utils:this.$Utils,
				pay:{}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					id:self.id
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						self.pay = {
							score:{
								price:parseFloat(self.mainData.price).toFixed(2)
							}
						}
					};
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
			
			
			goPay() {
				const self = this;	
				uni.setStorageSync('canClick',false);
				const postData = self.$Utils.cloneForm(self.pay)	
				postData.tokenFuncName = 'getProjectToken',
				postData.searchItem = {
					id: self.id
				};	
				const callback = (res) => {
					if (res.solely_code == 100000) {
						uni.setStorageSync('canClick', true);
						if (res.info) {
							const payCallback = (payData) => {
								console.log('payData', payData)
								if (payData == 1) {
									uni.showToast({
										title: '支付成功',
										duration: 1000,
										success: function() {
											
										}
									});
									setTimeout(function() {
										self.Router.redirectTo({route:{path:'/pages/orderPayOk/orderPayOk?id='+self.id}})
									}, 1000);
								} else {
									uni.setStorageSync('canClick', true);
									self.tips()
								};
							};
							self.$Utils.realPay(res.info, payCallback);
						} else {
							
							uni.showToast({
								title: '支付成功',
								duration: 1000,
								success: function() {
									
								}
							});
							setTimeout(function() {
								self.Router.redirectTo({route:{path:'/pages/orderPayOk/orderPayOk?id='+self.id}})
							}, 1000);
						};
					} else {
						uni.setStorageSync('canClick', true);
						uni.showToast({
							title: '调起支付失败',
							duration: 2000
						});
					};
				};
				self.$apis.pay(postData, callback);
			},
			
			tips(){
				const self = this;
				self.is_show = !self.is_show
				self.is_tips = !self.is_tips
			}
		}
	};
</script>

<style>
	page{background: #F5F5F5;padding-bottom: 140rpx;}
	.process .item{width: 50%;}
	.process .item .num{position: relative;}
	.process .item .num view{width: 36rpx;height: 36rpx;border-radius: 50%;background-color: #049d54;color: #fff;line-height: 36rpx;font-size: 22rpx;margin: 0 auto;margin-bottom: 20rpx;position: relative; z-index: 1;}
	
	.process {margin: 50rpx 0;}
	.process .item .num::after{content: ''; width: 50%;height: 1px;background-color: #ccc;position: absolute;right: 0;top: 50%;transform: translateY(-50%);z-index: 0;}
	.process .item:last-child .num::after{right:auto;left: 0;}
	.process .item.on .num::after{background-color: #049d54;}
	
	/* 弹框 */
	.tips-Show{width: 80%;position: fixed;top: 50%;left: 50%;transform: translate(-50%,-50%);z-index: 66;}
	.tips-Show .text{padding: 0 8%;}
	.tips-Show .btn{border-top: 1px solid #ddd;}
	
</style>
