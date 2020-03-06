<template>
	<view>
		<view class="mglr4 mgt15">
			<view class="pdlr4 HeadState flexRowBetween radius10">
				<view class="flex pubColor fs14"><image style="width: 30rpx; height: 30rpx;margin-right: 10rpx;" src="../../static/images/order-details-icon.png" mode=""></image>待提货</view>
				<view class="flexEnd">
					<view class="flexEnd">提货单号：<span class="pubColor">8</span></view>
				</view>
			</view>
			<view class="whiteBj radius10 pdlr4 mgt15">
				<view class="pdtb15 fs14 ">
					<view>收货人：张丹 15623565689</view>
					<view class="pdtb15 pubColor">自提点：门店名称门店名称</view>
					<view class="color6">地址：陕西省西安市高新区高新大都会A座</view>
				</view>
			</view>
			
			<view class="proRow mgt15">
				<view class="item">
					<view class="flexRowBetween">
						<view class="pic">
							<image src="../../static/images/shopping-img.png" mode=""></image>
						</view>
						<view class="infor">
							<view class="avoidOverflow fs14">智利车厘子 约200g/份 正负20g</view>
							<view class="B-price flexRowBetween">
								<view class="price fs16">18.8</view>
								<view class="fs13">×1</view>
							</view>
						</view>
					</view>
					<view class="flexEnd pdt15 fs13">共1件商品 实付金额:<span class="price fs15 ftw">56</span></view>
				</view>
			</view>	
		
			<view class="whiteBj radius10 pdlr4 mgt15">
				<view class="pdtb15 fs13 color6">
					<view class="mgb10">订单编号：5522256553326567</view>
					<view>下单时间：2020-03-05 17:20:40</view>
				</view>
			</view>
			
			<view class="fs13 color6 mgt20">1.如果您购买的商品有任何问题，请直接购买的门店联系！100%收货保障！</view>
			<view class="fs13 color6 mgt10">2.如果您找不到购物的提货门店，请致电牛小萌庄园帮忙热线！12356235862</view>
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
	.underBtn{border-top:0;padding-bottom: 0;}
	.HeadState{height: 143rpx;box-sizing: border-box;padding: 14rpx 4% 0 4%;background: url(../../static/images/order details-img.png) no-repeat 0 0/100% auto;}
	
</style>
