<template>
	<view v-if="showAll">
		
		<view>
			<image class="loginBj" src="../../static/images/login-img.png" mode=""></image>
		</view>
		
		<view class="loginCont fs13">
			<view class="item flex mgb15">
				<view class="icon"><image src="../../static/images/login-icon1.png" mode=""></image></view>
				<view class="input"><input type="text" v-model="submitData.login_name" placeholder="账号" placeholder-class="placeholder"></view>
			</view>
			<view class="item flex">
				<view class="icon"><image src="../../static/images/login-icon2.png" mode=""></image></view>
				<view class="input"><input type="password" v-model="submitData.password" placeholder="密码" placeholder-class="placeholder"></view>
			</view>
		</view>
		
		<view class="submitbtn" style="margin-top: 120rpx;">
			<button class="btn fs15" @click="submit">登录</button>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				submitData:{
					login_name:'',
					password:''
				},
				showAll:false
			}
		},
		
		onLoad() {
			const self = this;
			uni.hideLoading();
			if (uni.getStorageSync('merchantToken')) {
				uni.redirectTo({
					url: '/pages/business/business'
				})
			}else{
				self.showAll = true
			}
			// self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			submit() {
				const self = this;
				const postData = {
					login_name: self.submitData.login_name,
					password:self.submitData.password
				};
				if (self.$Utils.checkComplete(self.submitData)) {
					const callback = (res) => {
						if (res.solely_code == 100000) {
							console.log(res);
							uni.setStorageSync('merchantToken', res.token);
							uni.setStorageSync('merchantInfo', res.info);
							setTimeout(function() {
								self.Router.reLaunch({route:{path:'/pages/business/business'}})
							}, 1000);
						} else {
							self.$Utils.showToast(res.msg,'none')
						}
					}
					self.$apis.shopLogin(postData, callback);
				} else {
					self.$Utils.showToast('请补全登录信息', 'none')
				};
			},
		}
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
