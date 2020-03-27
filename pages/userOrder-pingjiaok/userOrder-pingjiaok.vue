<template>
	<view>
		
		<view class="mglr4">
			<view v-for="item in messageData">
				<view class="myOrderList fs12">
					<view class="item">
						<view class="flexRowBetween stateLine pdtb15 borderB1">
							<view>订单编号：{{item.product&&item.product.order_no?item.product.order_no:''}}</view>
							<view class="pubColor flexEnd">已完成</view>
						</view>
						<view class="flexRowBetween pdtb10 inforLine borderB1">
							<view class="picBox">
								<image :src="item.product&&item.product.orderItem&&item.product.orderItem[0]&&item.product.orderItem[0].snap_product&&
								item.product.orderItem[0].snap_product.mainImg&&item.product.orderItem[0].snap_product.mainImg[0]?
								item.product.orderItem[0].snap_product.mainImg[0].url:''" mode=""></image>
							</view>
							<!-- <view class="rr flexEnd">
								<image class="arrowR" src="../../static/images/about-icon.png" mode=""></image>
							</view> -->
						</view>
						<view class="flexRowBetween pdtb10">
							<view class="color9 fs13">{{item.product&&item.product.create_time?item.product.create_time:''}}</view>
							<view class="flexEnd">共{{item.product&&item.product.count?item.product.count:''}}件商品 合计:<span class="fs14">￥{{item.product&&item.product.price?item.product.price:''}}</span></view>
						</view>
					</view>
				</view>
				
				<view class="whiteBj radius10 pdlr4 pdt15 pdb15 mgt15">
					<view class="fs13">{{item.description}}</view>
					<view class="flex pjPic" v-if="item.bannerImg.length>0">
						<image v-for="c_item in item.bannerImg" :src="c_item.url" mode=""></image>
					</view>
					
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
				mainData:{},
				messageData:[]
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
				console.log(23232)
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					id: self.id,
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						self.getMessageData()
					};
					console.log('self.mainData', self.mainData)
					//self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
			
			getMessageData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					passage1:self.mainData.order_no
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.messageData.push.apply(self.messageData, res.info.data);
						for (var i = 0; i < self.messageData.length; i++) {
							for (var j = 0; j < self.mainData.child.length; j++) {
								if(self.messageData[i].order_no==self.mainData.child[j].order_no){
									self.messageData[i].product = self.mainData.child[j]
								}
							}
						}
					}
					console.log('self.messageData', self.messageData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.messageGet(postData, callback);
			},
			
		}
	};
</script>

<style>
	@import "../../assets/style/myOrder.css";
	page{background: #F5F5F5;padding-bottom: 60rpx;}
	.pjPic image{width: 190rpx;height: 190rpx;margin: 30rpx 30rpx 0 0;border-radius: 10rpx;}
	.pjPic image:nth-of-type(3n){margin-right: 0;}
</style>
