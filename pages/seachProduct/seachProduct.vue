<template>
	<view>
		<!-- 搜索 -->
		<view class="seachbox flexRowBetween whiteBj"  style="padding-bottom: 30rpx;"> 
			<view class="flex rr" style="width: 85%;">
				<button class="seachBtn" type="button"></button>
				<view class="input">
					<input type="text" v-model="keywords" placeholder="输入关键字搜索" placeholder-class="placeholder" />
				</view>
				<view class="delt flex"><text>×</text></view>
			</view>
			<view class="fs15 pubColor" @click="search">搜索</view>
		</view>
		<view class="mglr4 proList">
			<view class="item" v-for="(item,index) in mainData" :key="index">
				<view>
					<view class="center fs13 color6 mgb10">本商品由圣灵商行专供</view>
					<view class="pic">
						<image  v-if="item.mainImg&&item.mainImg[0]&&item.mainImg[0].type=='image'" :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image>
						<video style="height: 340rpx;" auto-pause-if-navigate="true" poster="http://106.12.155.217/nxm/public/uploads/2/20200310/jBDMCmvh1583822773.png" v-if="item.mainImg&&item.mainImg[0]&&item.mainImg[0].type=='vedio'" :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''"></video>
					</view>
					<view class="tit borderB1"   :data-id="item.id" @click="Router.navigateTo({route:{path:'/pages/productDetail/productDetail?id='+$event.currentTarget.dataset.id}})">{{item.title}}</view>
					<view class="data flex"   :data-id="item.id" @click="Router.navigateTo({route:{path:'/pages/productDetail/productDetail?id='+$event.currentTarget.dataset.id}})">
						<view class="mgr20">预售时间：{{Utils.timeto(item.start_time,'md')}}</view>
						<view>提货时间：{{Utils.timeto(item.end_time,'md')}}</view>
					</view>
					<view class="sellNum">今日已售<span class="pubColor">{{item.sale_count}}</span>份/限量{{item.sale_count+item.stock}}份</view>
				</view>
				<view class="flexRowBetween B-price">
					<view class="price fs18 ftw">{{item.price}}</view>
					<view></view>
					<view class="robBtn addCar" v-if="item.isStart&&item.count==0&&item.stock>0" @click="addToCar(index)">加入购物车</view>
					<view class=""  v-if="item.isStart&&item.count>0">
						<view class="numBox flex">
							<view class="btn" @click="counter('-',index)">-</view>
							<view class="num">{{item.count}}</view>
							<view class="btn add" @click="counter('+',index)">+</view>
						</view>
					</view>
					<view class="robBtn" v-if="item.stock==0&&item.isStart">抢光了</view>
					<view class="robBtn rob" v-if="!item.isStart">{{Utils.timeto(item.start_time,'h')}}点准时开抢</view>
				</view>
			</view>
		</view>
		
		<!-- 无数据 -->
		<view class="noDataBox" v-if="mainData.length==0">
			<image src="../../static/images/nodata.png" mode=""></image>
		</view>
		
		<view class="RFixIcon" @click="Router.switchTab({route:{path:'/pages/car/car'}})">
			<image src="../../static/images/search-icon.png" mode=""></image>
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				count:1,
				searchItem: {
					thirdapp_id: 2,
					on_shelf:1
				},
				mainData: [],
				allCount:0
			}
		},
		
		onLoad(options) {
			const self = this;
			if(options.keywords){
				self.keywords = options.keywords;
			}
			self.searchItem.title = ['like',['%'+self.keywords+'%']]
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			//self.$Utils.loadAll(['getMainData'], self);
		},
		
		
		
		onShow() {
			const self = this;
			self.getMainData()
			console.log('self.mainData',self.mainData)
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
			
			
			search(){
				const self =  this;
				self.getMainData(true)
			},
			
			addToCar(index){
				const self = this;
				self.mainData[index].count = 1;
				self.checkStatus()
			},
			
			counter(type,index) {
				const self = this;			
				if (type == '+') {
					self.mainData[index].count++;
				} else {
					if (self.count > 0) {
						self.mainData[index].count--;
					}
				};
				self.checkStatus()
			},
			
			checkStatus(){
				const self = this;
				self.allCount = 0;
				for (var i = 0; i < self.mainData.length; i++) {
					if(self.mainData[i].count>0){
						self.mainData[i].isCar = true;
						self.mainData[i].isSelect = true;
						self.$Utils.setStorageArray('cartData', self.mainData[i], 'id', 999);
						
					}else if(self.mainData[i].count==0&&self.mainData[i].isCar==true){
						console.log(i)
						self.mainData[i].isCar = false;
						self.mainData[i].isSelect = false;
						self.$Utils.delStorageArray('cartData', self.mainData[i], 'id');
					}
					self.$Utils.setStorageArray('indexData', self.mainData[i], 'id', 999);
				};
				
				console.log('cartData',self.$Utils.getStorageArray('cartData'))
				console.log('checkStatus',self.mainData)
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
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.order = {
					listorder: 'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData,res.info.data)
						var cartData = self.$Utils.getStorageArray('cartData')
						for (var i = 0; i < self.mainData.length; i++) {
							if(Date.parse(new Date())>parseInt(self.mainData[i].start_time)){
								self.mainData[i].isStart = true
							}else{
								self.mainData[i].isStart = false
							}
							for (var j = 0; j < cartData.length; j++) {
								if(self.mainData[i].id == cartData[j].id){
									self.mainData[i].count = cartData[j].count
								}
							}
						};
						self.checkStatus()
						console.log(22323)
					};
					self.$Utils.finishFunc('getMainData');	
				};
				self.$apis.productGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/seach.css";
	@import "../../assets/style/proList.css";
	@import "../../assets/style/numBox.css";
	page{padding-bottom: 40rpx;background: #F5F5F5;}
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
</style>

