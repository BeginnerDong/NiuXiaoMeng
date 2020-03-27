<template>
	<view>
		<view class="mglr4 mgt15">
			<view class="proRow">
				<view class="item" v-for="(item,index) in mainData.child" :key="index">
					<view class="flexRowBetween">
						<view class="pic">
							<image :src="item.orderItem&&item.orderItem[0]&&item.orderItem[0].snap_product
							&&item.orderItem[0].snap_product.mainImg&&item.orderItem[0].snap_product.mainImg[0]?item.orderItem[0].snap_product.mainImg[0].url:''"
							 mode=""></image>
						</view>
						<view class="infor">
							<view class="avoidOverflow fs14">{{item.title}}</view>
							<view class="B-price flexRowBetween">
								<view class="price fs16">{{item.unit_price}}</view>
								<view class="fs13">×{{item.count}}</view>
							</view>
						</view>
					</view>
					<view class="flexEnd pdt15 fs13">共{{item.count}}件商品 实付金额:<span class="price fs15 ftw">{{item.price}}</span></view>
					<view class="underBtn flexEnd" v-if="item.isremark==0">
						<view class="Bbtn" :data-id="item.id" @click="Router.navigateTo({route:{path:'/pages/userOrder-pingjia/userOrder-pingjia?id='+$event.currentTarget.dataset.id}})">去评价</view>
					</view>
					<view class="underBtn flexEnd" v-if="item.isremark==1">
						<view class="Bbtn">已评价</view>
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
				showView: false,
				score:'',
				wx_info:{},
				mainData:[{},{},{}],
				mainData:{}
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
					id: self.id,
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
					};
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
			
		}
	};
</script>

<style>
	@import "../../assets/style/myOrder.css";
	page{background: #F5F5F5;padding-bottom: 60rpx;}
	.underBtn{border-top:0;padding-bottom: 0;}
</style>
