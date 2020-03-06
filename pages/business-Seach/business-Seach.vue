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
			<view class="fs15 pubColor" @click="Router.redirectTo({route:{path:'/pages/business-SeachOrder/business-SeachOrder'}})">搜索</view>
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
				is_popupShow:false,
				keywords:''
			}
		},
		
		onLoad(options) {
			const self = this;
			console.log(uni.getStorageSync('historyDate'))
			if(uni.getStorageSync('historyDate')){
				self.historyDate = uni.getStorageSync('historyDate')
			};
			console.log(self.historyDate)
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		
		methods: {
			
			popupShow(){
				const self=this;
				self.is_popupShow = !self.is_popupShow;
				self.is_show = !self.is_show;
				
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
			
			clearHistory(){
				const self = this;
				self.historyDate = [];
				uni.removeStorageSync('historyDate');
				self.is_popupShow = !self.is_popupShow;
				self.is_show = !self.is_show;
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
	
	page{padding-bottom: 110rpx;background-color: #F5F5F5;}
	
</style>

