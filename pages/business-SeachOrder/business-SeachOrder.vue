<template>
	<view>
		<!-- 搜索 -->
		<view class="seachbox flexRowBetween whiteBj borderB1"  style="padding-bottom: 30rpx;"> 
			<view class="flex rr" style="width: 85%;">
				<button class="seachBtn" type="button"></button>
				<view class="input">
					<input type="text"  v-model="keywords" placeholder="搜索商品" placeholder-class="placeholder" />
				</view>
				<view class="delt flex"></view>
			</view>
			<view class="fs15 pubColor" @click="search">搜索</view>
		</view>
		<view class="mglr4">
			<view class="myOrderList fs12">
				<view class="item" v-for="(item,index) in mainData" :key="index">
					<view :data-id="item.id" @click="Router.navigateTo({route:{path:'/pages/businessOrder-detail/businessOrder-detail?id='+$event.currentTarget.dataset.id}})">
						<view class="flexRowBetween stateLine pdtb15 borderB1">
							<view>订单编号：{{item.order_no}}</view>
							<view class="pubColor flexEnd" v-if="item.transport_status==0">待提货</view>
							<view class="pubColor flexEnd" v-if="item.transport_status==2">已提货</view>
						</view>
						<view class="flexRowBetween pdtb10 inforLine borderB1">
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
				</view>
			</view>	
			
			<!-- 无数据 -->
			<view class="noDataBox">
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
				
				mainData:[],
				
				keywords:'',
				searchItem:{
					user_type:0,
					thirdapp_id:2,
					pay_status:1,
					level:1
				}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.keywords = options.keywords;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		
		methods: {
			
			search(){
				const self =  this;
				self.getMainData(true)
			},
			
			
			getMainData(isNew) {
				const self = this;
				console.log(2323)
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
				postData.tokenFuncName = 'getMerchantToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem)
				if(self.keywords!=''){
					postData.search = {}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData,res.info.data)
						for (var i = 0; i < self.mainData.length; i++) {
							self.mainData[i].totalCount = 0;
							for (var j = 0; j < self.mainData[i].child.length; j++) {
								self.mainData[i].totalCount += self.mainData[i].child[j].count
							}
						}
					}
					//self.$Utils.finishFunc('getMainData');
				};
				self.$apis.searchOrder(postData, callback);
			},
			
		}
	};
</script>

<style>
	@import "../../assets/style/seach.css";
	@import "../../assets/style/myOrder.css";
	
	page{padding-bottom: 110rpx;background-color: #F5F5F5;}
	
</style>

