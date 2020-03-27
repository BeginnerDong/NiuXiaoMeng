<template>
	<view>
		
		<view class="orderNav flex fs14 whiteBj color6 boxShaow">
			<view class="tt" :class="current==1?'on':''" @click="change('1')">全部</view>
			<view class="tt" :class="current==2?'on':''" @click="change('2')">未付款</view>
			<view class="tt" :class="current==3?'on':''" @click="change('3')">待提货</view>
			<view class="tt" :class="current==4?'on':''" @click="change('4')">已提货</view>
			<view class="tt" :class="current==5?'on':''" @click="change('5')">已评价</view>
		</view>
		<view class="pdtb20"></view>
		<view class="mglr4">
			<view class="myOrderList fs12">
				<view class="item" v-for="(item,index) in mainData" :key="index">
					<view>
						<view class="flexRowBetween stateLine pdtb15 borderB1">
							<view>订单编号：{{item.order_no}}</view>
							<view class="pubColor flexEnd" v-if="item.pay_status==0">待付款</view>
							<view class="pubColor flexEnd" v-if="item.pay_status==1&&item.transport_status==0">待提货</view>
							<view class="pubColor flexEnd" v-if="item.pay_status==1&&item.transport_status==2&&item.isremark==0">已提货</view>
							<view class="pubColor flexEnd" v-if="item.isremark==1">已评价</view>
						</view>
						<view class="flexRowBetween pdtb10 inforLine borderB1" :data-id="item.id"
						@click="Router.navigateTo({route:{path:'/pages/userOrder-detail/userOrder-detail?id='+$event.currentTarget.dataset.id}})">
							<view class="picBox flex"  v-for="(c_item,c_index) in item.child">
								<image :src="c_item.orderItem&&c_item.orderItem[0]&&c_item.orderItem[0].snap_product
							&&c_item.orderItem[0].snap_product.mainImg&&c_item.orderItem[0].snap_product.mainImg[0]?c_item.orderItem[0].snap_product.mainImg[0].url:''" mode=""></image>
							</view>
							<view class="rr flexEnd">
								<image class="arrowR" src="../../static/images/about-icon.png" mode=""></image>
							</view>
						</view>
						<view class="flexRowBetween pdtb10 borderB1">
							<view class="color9 fs13">{{item.create_time}}</view>
							<view class="flexEnd">共{{item.totalCount}}件商品 合计:<span class="fs14">￥{{item.price}}</span></view>
						</view>
						<view class="pdtb15 fs13" v-if="item.pay_status==1">提货单号：<span class="pubColor mgr5">{{item.code}}</span>{{item.phone}}</view>
					</view>
					<view class="underBtn flexEnd" v-if="item.pay_status==0">
						<view class="Bbtn"  :data-id="item.id"
						@click="Router.navigateTo({route:{path:'/pages/onlinePay/onlinePay?id='+$event.currentTarget.dataset.id}})">去付款</view>
					
					</view>
					<view class="underBtn flexEnd" v-if="item.pay_status==1&&item.transport_status==2&&item.isremark==0">
						<view class="Bbtn" :data-id="item.id"
						 @click="Router.navigateTo({route:{path:'/pages/userOrder-pingjiaList/userOrder-pingjiaList?id='+$event.currentTarget.dataset.id}})">去评价</view>
					</view>
					<view class="underBtn flexEnd" v-if="item.pay_status==1&&item.transport_status==2&&item.isremark==1">
						<view class="Bbtn gryBtn" :data-id="item.id"
						@click="Router.navigateTo({route:{path:'/pages/userOrder-pingjiaok/userOrder-pingjiaok?id='+$event.currentTarget.dataset.id}})">查看评价</view>
					</view>
				</view>
			</view>
			
			<!-- 无数据 -->
			<view class="noDataBox" v-if="mainData.length==0">
				<image src="../../static/images/nodata.png" mode=""></image>
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
				wx_info:{},
				is_show:false,
				current:1,
				searchItem:{
					level:1
				},
				mainData:[]
			}
		},
		
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			//self.$Utils.loadAll(['getMainData'], self);
		
		},
		
		onShow() {
			const self = this;
			self.getMainData(true)
		},
		
		
		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},
		
		methods: {
			
			change(current) {
				const self = this;
				if(current!=self.current){
					self.current = current;
					if(self.current==1){
						delete self.searchItem.pay_status;
						delete self.searchItem.transport_status;
						delete self.searchItem.isremark;
					}else if(self.current==2){
						delete self.searchItem.isremark;
						self.searchItem.pay_status=0
					}else if(self.current==3){
						delete self.searchItem.isremark;
						self.searchItem.pay_status=1
						self.searchItem.transport_status=0
					}else if(self.current==4){
						self.searchItem.isremark=0
						self.searchItem.pay_status=1
						self.searchItem.transport_status=2
					}else if(self.current==5){
						self.searchItem.pay_status=1
						self.searchItem.transport_status=2
						self.searchItem.isremark=1
					}
					self.getMainData(true)
				}
			},
			
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						is_page: true,
						pagesize: 10
					}
				};
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
						for (var i = 0; i < self.mainData.length; i++) {
							self.mainData[i].totalCount = 0;
							for (var j = 0; j < self.mainData[i].child.length; j++) {
								self.mainData[i].totalCount += self.mainData[i].child[j].count
							}
						}
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/myOrder.css";
	page{background: #F5F5F5;padding-bottom: 140rpx;}
	.orderNav{position: fixed;left: 0;top: 0rpx;right: 0; z-index: 5;}
	
</style>
