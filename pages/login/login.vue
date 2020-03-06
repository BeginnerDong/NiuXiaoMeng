<template>
	<view>
		
		<view>
			<image class="loginBj" src="../../static/images/login-img.png" mode=""></image>
		</view>
		
		<view class="loginCont fs13">
			<view class="item flex mgb15">
				<view class="icon"><image src="../../static/images/login-icon1.png" mode=""></image></view>
				<view class="input"><input type="text" placeholder="账号" placeholder-class="placeholder"></view>
			</view>
			<view class="item flex">
				<view class="icon"><image src="../../static/images/login-icon2.png" mode=""></image></view>
				<view class="input"><input type="password" placeholder="密码" placeholder-class="placeholder"></view>
			</view>
		</view>
		
		<view class="submitbtn" style="margin-top: 120rpx;">
			<button class="btn fs15" @click="Router.navigateTo({route:{path:'/pages/business/business'}})">登录</button>
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


		},
	};
</script>

<style>
	.loginBj{width: 100%;height: 330rpx;}
	.userHead{box-sizing: border-box;padding: 50rpx 4%;height: 306rpx;}
	.userHead .photo{width: 120rpx;height: 120rpx;border-radius: 50%;margin-right: 20rpx;box-sizing: border-box;}
	.loginCont{margin: 80rpx 10% 0 10%;}
	.loginCont .item{padding: 0 30rpx;height: 80rpx;background-color: #f5f5f5;border-radius: 10rpx;}
	.loginCont .item .icon{width: 34rpx;height: 34rpx;margin-right: 20rpx;}
	.loginCont .item .icon image{width: 100%;height: 100%;}
	.loginCont .item .input{width: 87%;}
	.loginCont .item .input input{width: 100%;height: 60rpx;display: block;font-size: 28rpx;}
</style>
