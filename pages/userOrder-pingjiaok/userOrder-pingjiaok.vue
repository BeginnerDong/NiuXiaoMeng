<template>
	<view>
		
		<view class="mglr4">
			<view class="myOrderList fs12">
				<view class="item">
					<view class="flexRowBetween stateLine pdtb15 borderB1">
						<view>订单编号：15558755542525</view>
						<view class="pubColor flexEnd">已完成</view>
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
				<view class="fs13">各说各话融合法律的后果绿色个人空间的个人鼓风机肯定会right更好看的高房价的控股股份的咖啡店</view>
				<view class="flex pjPic">
					<image src="../../static/images/evaluation-img1.png" mode=""></image>
					<image src="../../static/images/evaluation-img1.png" mode=""></image>
					<image src="../../static/images/evaluation-img1.png" mode=""></image>
				</view>
				
			</view>
			
			
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
	.pjPic image{width: 190rpx;height: 190rpx;margin: 30rpx 30rpx 0 0;border-radius: 10rpx;}
	.pjPic image:nth-of-type(3n){margin-right: 0;}
</style>
