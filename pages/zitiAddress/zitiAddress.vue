<template>
	<view>
			<view class="myaddress-lis" v-for="(item,index) in mainData" :key="index">
				<view class="flex addBox">
					<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" class="mdImg"></image>
					<view class="flex-1 inforBox">
						<view class="name" @click="choose(index)">{{item.name}}</view>
						<view class="adrs" @click="choose(index)">地址：{{item.address}}</view>
					</view>
				</view>
				
				<view class="seltBox">
					<view class="L"  @click="setDefault(index)">
						<image class="icon" :src="defaultNo == item.user_no?'../../static/images/stores-icon.png':'../../static/images/stores-icon1.png'" mode=""></image>
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
				
				mainData:[],
				defaultNo:''
			}
		},

		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.defaultNo = uni.getStorageSync('user_info').info.shop_no;
			self.$Utils.loadAll(['getLocation'], self);
		},

		
		
		/* onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		}, */
		

		methods: {
			
			setDefault(index) {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					user_no:uni.getStorageSync('user_info').user_no
				};
				postData.data = {
					shop_no:self.mainData[index].user_no
				};
				const callback = (data) => {				
					if (data.solely_code == 100000) {					
						self.defaultNo = self.mainData[index].user_no
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}	
				};
				self.$apis.userInfoUpdate(postData, callback);
			},
			
			getLocation(){
				const self = this;
				uni.getLocation({
				    type: 'wgs84',
				    success: function (res) {
						self.getMainData(res.latitude,res.longitude)
				        console.log('当前位置的经度：' + res.longitude);
				        console.log('当前位置的纬度：' + res.latitude);
				    }
				});
			},
			
			getMainData(latitude,longitude) {
				const self = this;
				self.nearShopData = [];
				const postData = {
					tokenFuncName:'getProjectToken',
					longitude:longitude,
					latitude:latitude,
				};
				const callback = (res) => {
					if (res.info.length > 0) {
						self.mainData.push.apply(self.mainData,res.info)
					}
					self.$Utils.finishFunc('getLocation');
				};
				self.$apis.getShop(postData, callback);
			},
			
		
		}
	}
</script>

<style>
	/* @import "../../assets/style/seltAlert.css"; */
	page{ background: #f5f5f5;}
	.myaddress-lis{padding:30rpx 4%;margin-bottom: 20rpx;background: #fff; }
	.myaddress-lis .name{ font-size: 28rpx; color: #222;line-height: 1;padding-top: 5rpx;}
	.myaddress-lis .numb{margin-left: 30rpx; width: 300rpx; display: inline-block;}
	.myaddress-lis .adrs{ font-size: 26rpx;color: #666; line-height: 40rpx;padding: 10rpx 0 0;}
	.seltBox{ display: flex;justify-content: space-between; align-items: center;padding-top: 20rpx;}
	.seltBox .L{ width: 30%; display: flex; align-items: center; font-size: 24rpx; color: #999;}
	.seltBox .L .icon{ width: 30rpx; height: 30rpx; display: inline-block; margin-right: 10rpx;}
	.seltBox .R{ width: 70%;}
	.seltBox .R image{  width:30rpx; height: 30rpx; display:block; margin-right: 8rpx;}
	
	.addBox{border-bottom: 1px solid #e1e1e1;padding-bottom: 20rpx;}
	.inforBox{flex-direction: column;display: flex;justify-content: space-between;height: 120rpx;}
</style>
