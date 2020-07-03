<template>
	<view>
		
		<view class="pdlr4 pdt15 whiteBj">
			<view class="seachBox flexCenter color9 fs12" @click="Router.navigateTo({route:{path:'/pages/seach/seach'}})">
				<image class="icon" src="../../static/images/home-icon.png" mode=""></image>
				<view>搜索</view>
			</view>
		</view>
		
		<view class="indHome flex whiteBj pdt15" style="min-height: 320rpx;">
			<view class="item" v-for="(item,index) in labelData" :data-id="item.id" :key="index" 
			@click="Router.navigateTo({route:{path:'/pages/productList/productList?id='+$event.currentTarget.dataset.id}})">
				<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''"></image>
				<view class="tit avoidOverflow">{{item.title}}</view>
			</view>
		</view>
		<view class="f5H10" @click="flowLogAdd"></view>
		
		<button open-type="contact">
			<view class="RFixIcon" style="bottom: 20%;">
				<image src="../../static/images/home-icon9.png" mode=""></image>
			</view>
		</button>
		
		<button open-type="share">
			<view class="RFixIcon" style="bottom: 10%;">
				<image src="../../static/images/share-icon1.png" mode=""></image>
			</view>
		</button>
		
		<view class="radius10 whiteBj mgt10 pdlr4 pdb15 mglr4">
			<view class="flexRowBetween pdt15 pdb10 borderB1" @click="Router.navigateTo({route:{path:'/pages/zitiAddress/zitiAddress'}})">
				<view class="ftw">店铺自提点</view>
				<view class="flexEnd fs12 color9" >更换自提点<image class="arrowR" src="../../static/images/about-icon.png" mode=""></image></view>
			</view>
			<view class="flex pdt10">
				<image :src="shopData.mainImg&&shopData.mainImg[0]?shopData.mainImg[0].url:''" class="mdImg"></image>
				<view class="flex-1">
					<view class="pubColor pdb10">{{shopData.name?shopData.name:''}}</view>
					<view class="fs13 color6 pdt5">地址：{{shopData.address?shopData.address:''}}</view>
				</view>
			</view>
		</view>
		
		
		<view class="mglr4 proList">
			<view class="item" v-for="(item,index) in mainData" :key="index">
				<view>
					<!-- <view class="center fs13 color6 mgb10">本商品由圣灵商行专供</view> -->
					<view class="pic">
						<image  v-if="item.mainImg&&item.mainImg[0]&&item.mainImg[0].type=='image'" :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image>
						<video style="height: 340rpx;width: 100%;" auto-pause-if-navigate="true"  v-if="item.mainImg&&item.mainImg[0]&&item.mainImg[0].type=='vedio'" :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''"></video>
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
		
		
		<!--底部tab键-->
		<!-- <view class="navbar">
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar1-a.png" />
				</view>
				<view class="text this-text">首页</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/car/car'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar2.png" />
				</view>
				<view class="text">购物车</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/user/user'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar3.png" />
				</view>
				<view class="text">我的</view>
			</view>
		</view> -->
		<!--底部tab键 end-->
		
	</view>
	
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				count:1,
				labelData: [],
				searchItem: {
					thirdapp_id: 2,
					on_shelf:1,
					
				},
				mainData: [],
				allCount:0,
				shopData:{}
			}
		},
		
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getLabelData'], self);
		},
		
		onShow() {
			const self = this;
			if(self.$Utils.getStorageArray('indexData')&&self.$Utils.getStorageArray('indexData').length>0){
				self.mainData = self.$Utils.getStorageArray('indexData');
				for (var i = 0; i < self.mainData.length; i++) {
					if(Date.parse(new Date())>parseInt(self.mainData[i].start_time)){
						self.mainData[i].isStart = true
					}else{
						self.mainData[i].isStart = false
					}
				};
				self.checkStatus()
			}else{
				self.getMainData()
			}
			self.getUserInfoData()
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
		
		onShareAppMessage(ops) {
			console.log(ops)
			const self = this;
			if (ops.from === 'button') {
				
				return {
					title:'牛小萌庄园',
					path: '/pages/index/index', //点击分享的图片进到哪一个页面
					//imageUrl:self.mainData&&self.mainData.mainImg&&self.mainData.mainImg[0]&&self.mainData.mainImg[0].url?self.mainData.mainImg[0].url:'',
					success: function(res) {
						// 转发成功
						
						console.log("转发成功:" + JSON.stringify(res));
					},
					fail: function(res) {
						// 转发失败
						console.log("转发失败:" + JSON.stringify(res));
					}
				}
			}else{
				return {
					title:'牛小萌庄园',
					path: '/pages/index/index', //点击分享的图片进到哪一个页面
					//imageUrl:self.mainData&&self.mainData.mainImg&&self.mainData.mainImg[0]&&self.mainData.mainImg[0].url?self.mainData.mainImg[0].url:'',
					success: function(res) {
						// 转发成功
						
						console.log("转发成功:" + JSON.stringify(res));
					},
					fail: function(res) {
						// 转发失败
						console.log("转发失败:" + JSON.stringify(res));
					}
				}
			}
		},
		
		methods: {
			
			getUserInfoData() {
				const self = this;
				const postData = {
					searchItem:{thirdapp_id:2}
				};
				postData.tokenFuncName = 'getProjectToken'
				postData.searchItem.user_no = uni.getStorageSync('user_info').user_no;
				postData.getAfter = {
					shop:{
						tableName:'UserInfo',
						middleKey:'shop_no',
						key:'user_no',
						searchItem:{status:1},
						condition:'='
					},
				};
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.userInfoData = res.info.data[0];
						if(res.info.data[0].shop[0]){
							self.shopData = res.info.data[0].shop[0]
						}else{
							self.getLocation()
						}
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
					// self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			getLocation(){
				const self = this;
				uni.getLocation({
				    type: 'wgs84',
				    success: function (res) {
						self.getNearShop(res.latitude,res.longitude)
				        console.log('当前位置的经度：' + res.longitude);
				        console.log('当前位置的纬度：' + res.latitude);
				    }
				});
			},
			
			getNearShop(latitude,longitude) {
				const self = this;
				self.nearShopData = [];
				const postData = {
					tokenFuncName:'getProjectToken',
					longitude:longitude,
					latitude:latitude,
				};
				const callback = (res) => {
					if (res.info.length > 0) {
						self.shopData = res.info[0]
					}
				};
				self.$apis.getShop(postData, callback);
			},
			
			flowLogAdd() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.data = {
					count:10000,
					thirdapp_id:2,
					status:1,
					trade_info:'赠送',
					type:3,
					account:1,
				};
				const callback = (data) => {				
					if (data.solely_code == 100000) {					
						
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}	
				};
				self.$apis.flowLogAdd(postData, callback);
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
				console.log('counter',self.mainData)
				//if(self.mainData[index].count)
				self.checkStatus()
			},
			
			checkStatus(){
				const self = this;
				self.allCount = 0;
				for (var i = 0; i < self.mainData.length; i++) {
					if(self.mainData[i].count>0){
						self.mainData[i].isCar = true;
						self.mainData[i].isSelect = true;
						self.allCount += self.mainData[i].count;
						self.$Utils.setStorageArray('cartData', self.mainData[i], 'id', 999);
						uni.setTabBarBadge({
						  index: 1,
						  text: self.allCount.toString()
						})
						
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
						for (var i = 0; i < res.info.data.length; i++) {
							res.info.data[i].count = 0
							res.info.data[i].isCar = false;
							res.info.data[i].isSelect = false;
							
							self.$Utils.setStorageArray('indexData', res.info.data[i], 'id', 999);
						}
						
						self.mainData = self.$Utils.getStorageArray('indexData');
						for (var i = 0; i < self.mainData.length; i++) {
							if(Date.parse(new Date())>parseInt(self.mainData[i].start_time)){
								self.mainData[i].isStart = true
							}else{
								self.mainData[i].isStart = false
							}
						};
						self.checkStatus()
						console.log(22323)
					};
					self.$Utils.finishFunc('getMainData');	
				};
				self.$apis.productGet(postData, callback);
			},
			
			
			getLabelData() {
				var self = this;
				var postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					type: 3
				};
				postData.order = {
					listorder: 'desc'
				};
				var callback = function(res) {
					if (res.info.data.length > 0 && res.info.data[0]) {
						self.labelData.push.apply(self.labelData, res.info.data)
					};
					self.$Utils.finishFunc('getLabelData');
				};
				self.$apis.labelGet(postData, callback);
			},
			
		}
	};
</script>

<style>
	@import "../../assets/style/navbar.css";
	@import "../../assets/style/proList.css";
	@import "../../assets/style/numBox.css";
	
	page{padding-bottom: 140rpx;background-color: #F5F5F5;}	
	
	.seachBox{border-radius: 30rpx;background: #f5f5f5;padding:10rpx 80rpx 10rpx 30rpx;position: relative;box-sizing: border-box;}
	.seachBox input{width: 100%;min-height: auto;line-height: 40rpx;height: 40rpx;display: block;font-size: 26rpx;}
	.seachBox .icon{width: 30rpx;height: 30rpx;margin-right: 10rpx;}
	.seachBox .rr{width: 60rpx;height: 40rpx;position: absolute; right: 10rpx;top: 50%;transform: translateY(-50%);}	
	.seachBox .btn{width:100%;height: 100%;background: url(../../static/images/home-icon.png) no-repeat center center /30rpx 30rpx;}
	
	/* 分类导航 */
	.indHome{ flex-wrap:wrap;}
	.indHome .item{width: 25%;text-align: center;padding-bottom: 36rpx; font-size: 26rpx; color: #222;display: flex; flex-direction: column;}
	.indHome .item image{width:90rpx;height:90rpx; margin: 0 auto 16rpx auto; }
	
	
	
</style>
