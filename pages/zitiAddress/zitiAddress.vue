<template>
	<view>
			<view class="myaddress-lis" v-for="(item,index) in myaddressDate" :key="index">
				<view class="name">门店名称门店名称门店名称门店名称</view>
				<view class="adrs">地址：陕西省西安市高新区大都荟</view>
				<view class="seltBox">
					<view class="L"  @click="seltCurr(index)">
						<image class="icon" :src="curr == index?'../../static/images/stores-icon.png':'../../static/images/stores-icon1.png'" mode=""></image>
						默认地址
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
				is_show:false,
				wx_info: {},
				curr:0,
				myaddressDate:[{},{},{},{}]
			}
		},

		onLoad(options) {
			uni.setStorageSync('canClick', true);
		},

		onShow() {
			const self = this;
			document.title = ''
		},

		methods: {
			seltCurr(index){
				const self = this;
				self.curr = index
			},
			getMainData() {
				const self = this;
				self.$apis.userGet(postData, callback);
			}
		}
	}
</script>

<style>
	/* @import "../../assets/style/seltAlert.css"; */
	page{ background: #f5f5f5;}
	.myaddress-lis{padding:30rpx 4%;margin-bottom: 20rpx;background: #fff; }
	.myaddress-lis .name{ font-size: 28rpx; color: #222;}
	.myaddress-lis .numb{margin-left: 30rpx; width: 300rpx; display: inline-block;}
	.myaddress-lis .adrs{ font-size: 26rpx;color: #666; line-height: 40rpx;padding: 10rpx 0;}
	.seltBox{ display: flex;justify-content: space-between; align-items: center;padding-top: 10rpx;}
	.seltBox .L{ width: 30%; display: flex; align-items: center; font-size: 24rpx; color: #999;}
	.seltBox .L .icon{ width: 30rpx; height: 30rpx; display: inline-block; margin-right: 10rpx;}
	.seltBox .R{ width: 70%;}
	.seltBox .R image{  width:30rpx; height: 30rpx; display:block; margin-right: 8rpx;}

</style>
