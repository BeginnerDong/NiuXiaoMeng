<template>
	<view>
		<!-- 搜索 -->
		<view class="seachbox flexRowBetween whiteBj borderB1"  style="padding-bottom: 30rpx;"> 
			<view class="flex rr" style="width: 85%;" @click="Router.navigateTo({route:{path:'/pages/business-Seach/business-Seach'}})">
				<button class="seachBtn" type="button"></button>
				<view class="input">
					<input type="text"  v-model="keywords" disabled="true" placeholder="搜索单号、手机号、收货人" placeholder-class="placeholder" />
				</view>
				<view class="delt flex"></view>
			</view>
			<view class="fs15 pubColor" @click="scanCode"><image class="scanBtn" src="../../static/images/merchants-icon.png" mode=""></image></view>
		</view>
		
		<view class="myRowBetween mgt10 whiteBj">
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/businessOrder/businessOrder?type=all'}})">
				<view class="ll">全部订单</view>
				<view class="rr"><image class="arrowR" src="../../static/images/about-icon.png" mode=""></image></view>
			</view>
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/businessOrder/businessOrder?type=month'}})">
				<view class="ll">当月订单</view>
				<view class="rr"><image class="arrowR" src="../../static/images/about-icon.png" mode=""></image></view>
			</view>
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/businessOrder/businessOrder?type=day'}})">
				<view class="ll">当日订单</view>
				<view class="rr"><image class="arrowR" src="../../static/images/about-icon.png" mode=""></image></view>
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
				mainData:[]
			}
		},
		
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
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
			
			clickSearch(item){
				const self = this;
				self.Router.navigateTo({route:{path:'/pages/seachProduct/seachProduct?keywords='+item}});
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
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					thirdapp_id:2
				};
				postData.order = {
					count:'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData,res.info.data)
					}
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.hotSearchGet(postData, callback);
			},
			
		}
	};
</script>

<style>
	@import "../../assets/style/seach.css";
	@import "../../assets/style/editInfor.css";
	page{padding-bottom: 110rpx;background-color: #F5F5F5;}
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
	.scanBtn{width:60rpx;height: 60rpx;}
	.myRowBetween .item{padding: 30rpx 4%;}
</style>

