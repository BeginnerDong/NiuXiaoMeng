<template>
	<view>
		<view class="headTit center pr white">
			<image src="../../static/images/xiadan-icon1.png" mode=""></image>
			<view class="tit fs18">您的提货单号：9</view>
		</view>
		
		<view class="pdlr4 okCont">
			<view class="okIcon"><image src="../../static/images/xiadan-icon.png" mode=""></image></view>
			<view class="center mgb30">支付成功</view>
			
			<view class="seltBtn flexRowBetween fs14 color2 center" style="width:85%;margin:0 auto ;">
				<view class="btn" @click="Router.switchTab({route:{path:'/pages/index/index'}})">继续购物</view>
				<view class="btn on">提醒老板接单</view>
			</view>
			<view class="fs12 mgt20 color6">点击上面的<span class="pubColor">“提醒老板接单”</span>按钮，将提醒消息发送到本店微信群：<span class="pubColor">“兴盛优选问309”</span>群，提醒老板及时处理您的订单</view>
			
			<view class="fs12 color9 mgt25">安全提示：牛小萌庄园不会以任何理由要求您提供银行卡信息或支付额外费用，请谨防钓鱼链接或咋骗电话</view>
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
				is_show:false
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
	page{padding-bottom: 60rpx;}
	.headTit image{width: 100%;height: 270rpx;}
	.headTit .tit{position: absolute;top: 100rpx;left: 4%;right: 4%;z-index: 1;}
	.okIcon{width: 231rpx;height: 220rpx;margin: 60rpx auto 20rpx auto;}
	.okIcon image{width: 100%;height: 100%;}
	
	.seltBtn .btn{width: 260rpx;height: 70rpx;line-height: 70rpx;border-radius:40rpx;border:1px solid #ccc;}
	.seltBtn .btn.on{border-color: #E1211C;color: #E1211C;}
	
</style>
