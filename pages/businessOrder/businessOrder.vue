<template>
	<view>
		<!-- 搜索 -->
		<view style="position: fixed;left: 0;top: 88rpx;right: 0; z-index: 5;">
			<view class="seachbox flexRowBetween whiteBj borderB1"  style="padding-bottom: 30rpx;"> 
				<view class="flex rr" style="width: 85%;" @click="Router.redirectTo({route:{path:'/pages/business-Seach/business-Seach'}})">
					<button class="seachBtn" type="button"></button>
					<view class="input">
						<input type="text"  v-model="keywords" placeholder="搜索单号、手机号、收货人" placeholder-class="placeholder" />
					</view>
					<view class="delt flex"></view>
				</view>
				<view class="fs15 pubColor" @click="Router.redirectTo({route:{path:'/pages/businessOrder-detail/businessOrder-detail'}})"><image class="scanBtn" src="../../static/images/merchants-icon.png" mode=""></image></view>
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
				<view class="item">
					<view @click="Router.navigateTo({route:{path:'/pages/businessOrder-detail/businessOrder-detail'}})">
						<view class="flexRowBetween stateLine pdtb15 borderB1">
							<view>订单编号：15558755542525</view>
							<view class="pubColor flexEnd">待提货</view>
						</view>
						<view class="flexRowBetween pdtb10 inforLine borderB1">
							<view class="picBox flex">
								<image src="../../static/images/evaluation-img.png" mode=""></image>
							</view>
							<view class="rr flexEnd">
								<image class="arrowR" src="../../static/images/about-icon.png" mode=""></image>
							</view>
						</view>
						<view class="flexRowBetween pdtb10 borderB1">
							<view class="color9 fs13">2020.03.04 14:30:20</view>
							<view class="flexEnd">共1件商品 合计:<span class="fs14">￥56</span></view>
						</view>
						<view class="pdtb15 fs13">提货单号：<span class="pubColor mgr5">3454</span>15623565689</view>
					</view>
				</view>
				<view class="item">
					<view @click="Router.navigateTo({route:{path:'/pages/businessOrder-detail/businessOrder-detail'}})">
						<view class="flexRowBetween stateLine pdtb15 borderB1">
							<view>订单编号：15558755542525</view>
							<view class="pubColor flexEnd">已提货</view>
						</view>
						<view class="flexRowBetween pdtb10 inforLine borderB1">
							<view class="picBox flex">
								<image src="../../static/images/evaluation-img.png" mode=""></image>
								<image src="../../static/images/evaluation-img.png" mode=""></image>
								<image src="../../static/images/evaluation-img.png" mode=""></image>
							</view>
							<view class="rr flexEnd">
								<image class="arrowR" src="../../static/images/about-icon.png" mode=""></image>
							</view>
						</view>
						<view class="flexRowBetween pdtb10 borderB1">
							<view class="color9 fs13">2020.03.04 14:30:20</view>
							<view class="flexEnd">共1件商品 合计:<span class="fs14">￥56</span></view>
						</view>
						<view class="pdtb15 fs13">提货单号：<span class="pubColor mgr5">3454</span>15623565689</view>
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
				showView: false,
				wx_info:{},
				is_show:false,
				mainData:[],
				current:1
			}
		},
		
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			change(current) {
				const self = this;
				if(current!=self.current){
					self.current = current
				}
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
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/myOrder.css";
	page{padding-bottom: 110rpx;background-color: #F5F5F5;}
	.scanBtn{width:60rpx;height: 60rpx;}
	/* .orderNav{position: fixed;left: 0;top: 88rpx;right: 0; z-index: 5;} */
</style>

