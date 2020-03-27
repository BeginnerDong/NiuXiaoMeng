<template>
	<view>
		<!-- 搜索 -->
		<view style="position: fixed;left: 0;right: 0; z-index: 5;">
			<view class="seachbox flexRowBetween whiteBj borderB1"  style="padding-bottom: 30rpx;"> 
				<view class="flex rr" style="width: 85%;" @click="Router.redirectTo({route:{path:'/pages/business-Seach/business-Seach'}})">
					<button class="seachBtn" type="button"></button>
					<view class="input">
						<input type="text"  disabled="true" placeholder="搜索单号、手机号、收货人" placeholder-class="placeholder" />
					</view>
					<view class="delt flex"></view>
				</view>
				<view class="fs15 pubColor" @click="scanCode"><image class="scanBtn" src="../../static/images/merchants-icon.png" mode=""></image></view>
			</view>
			
			<view class="orderNav flex fs14 whiteBj color6 boxShaow">
				<view class="tt" :class="current==1?'on':''" @click="change('1')">全部</view>
				<view class="tt" :class="current==2?'on':''" @click="change('2')">待提货</view>
				<view class="tt" :class="current==3?'on':''" @click="change('3')">已提货</view>
			</view>
		</view>
		<view style="height: 202rpx;"></view>
		
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
				keywords:'',
				mainData:[],
				current:1,
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
			console.log(2323)
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			var data = new Date(); //本月
			data.setDate(1);
			data.setHours(0);
			data.setSeconds(0);
			data.setMinutes(0);
			var monthStart = parseInt(data.getTime() / 1000);
			var dayStart = new Date(new Date().setHours(0, 0, 0, 0)).getTime() / 1000;
			var nowTime = Date.parse(new Date()) / 1000;
			self.type = options.type;
			self.searchItem.shop_no = uni.getStorageSync('merchantInfo').user_no;
			if(self.type=='month'){
				self.searchItem.create_time = ['between',[monthStart,nowTime]]
			}else if(self.type=='day'){
				self.searchItem.create_time = ['between',[dayStart,nowTime]]
			};
			console.log(2323)
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
			
			scanCode(){
				const self = this;
				uni.scanCode({
				    success: function (res) {
				        self.getOrderData(res.result)
				    }
				});
			},
			
			getOrderData(order_no) {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getMerchantToken';
				postData.searchItem = {
					order_no:order_no,
					user_type:0
				};
				
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.Router.navigateTo({route:{path:'/pages/businessOrder-detail/businessOrder-detail?id='+res.info.data[0].id}})
					}else{
						self.$Utils.showToast('二维码无效')
					}
				};
				self.$apis.orderGet(postData, callback);
			},
			
			change(current) {
				const self = this;
				if(current!=self.current){
					self.current = current;
					if(self.current==1){
						delete self.searchItem.transport_status
					}else if(self.current==2){
						self.searchItem.transport_status = 0
					}else if(self.current==3){
						self.searchItem.transport_status = 2
					};
					self.getMainData(true)
				}
			},
			
			
			
			search(){
				const self = this;
				if(self.keywords!=''){
					self.Router.navigateTo({route:{path:'/pages/seachProduct/seachProduct?keywords='+self.keywords}});
					self.historyDate.push(self.keywords);
					uni.setStorageSync('historyDate',self.historyDate);
					self.keywords = '';
				}else{
					return
				}
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
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/myOrder.css";
	page{padding-bottom: 110rpx;background-color: #F5F5F5;}
	.scanBtn{width:60rpx;height: 60rpx;}
	button{
		background: none;
		line-height: 1.5;
		margin-left: 0;
		margin-right: 0;
		font-size: 16px;
		border-radius: 0;
	}
	button::after{
		border: none;
	}
	.button-hover {
		color: #000000;
		background: none;
	}
	/* .orderNav{position: fixed;left: 0;top: 88rpx;right: 0; z-index: 5;} */
</style>

