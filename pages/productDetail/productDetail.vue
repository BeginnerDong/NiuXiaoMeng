<template>
	<view>
		
		<view class="detailxqBan">
			<image :src="mainData.bannerImg&&mainData.bannerImg[0]?mainData.bannerImg[0].url:''" mode=""></image>
		</view>
		<view class="mglr4 pdb15 detailHead">
			<view class="tit borderB1">{{mainData.title}}</view>
			<view class="data flex">
				<view class="mgr20">预售时间：{{Utils.timeto(mainData.start_time,'md')?Utils.timeto(mainData.start_time,'md'):''}}</view>
				<view>提货时间：{{Utils.timeto(mainData.end_time,'md')?Utils.timeto(mainData.end_time,'md'):''}}</view>
			</view>
			<view class="fs13 color6">今日已售<span class="pubColor">{{mainData.sale_count}}</span>份/限量{{mainData.sale_count+mainData.stock}}份</view>
			<view class="pdt15">
				<view class="price fs18 ftw">{{mainData.price}}</view>
			</view>
		</view>
		<view class="f5H10"></view>
		<view class="mglr4 pdt20 pdb10 xqInfor">
			<view class="fs15 ftw mgb10">商品规格描述</view>
			<view class="cont">
				<view class="content ql-editor" style="padding:0;"
				v-html="mainData.content">
				</view>
			</view>
		</view>
		
		<!-- 评价 -->
		<view class="pdlr4" style="border-top: 20rpx solid #F5F5F5;">
			<view class="flexRowBetween pdt15 pdb10">
				<view class="fs15 ftw">商品评价</view>
				<view class="fs13 color9 flexEnd"  @click="Router.navigateTo({route:{path:'/pages/pingjia/pingjia'}})">查看更多<image class="arrowR" src="../../static/images/about-icon.png" mode=""></image></view>
			</view>
			<view class="detail_pjLis">
				<view class="item" v-for="(item,index) in messageData" :key="index">
					<view class="photo"><image src="../../static/images/details-img2.png" mode=""></image></view>
					<view class="cont">
						<view class="flexRowBetween pdt5 pdb10">
							<view class="name font14">哆啦A梦</view>
							<view class="time fs12 color6">2020.03.04</view>
						</view>
						<view class="text">根据瑞灵活赶紧来都搞好了电话沟通各环节的反馈给换了多个黑客帝国</view>
						<view class="picLis flex">
							<view class="pic">
								<image src="../../static/images/evaluation-img1.png" mode=""></image>
							</view>
							<view class="pic">
								<image src="../../static/images/evaluation-img1.png" mode=""></image>
							</view>
							<view class="pic">
								<image src="../../static/images/evaluation-img1.png" mode=""></image>
							</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		
		<!-- 底部菜单按钮 -->
		<view class="xqbotomBar" v-if="mainData.isStart&&mainData.stock>0">
			<view class="left">
				<view class="ite" @click="Router.switchTab({route:{path:'/pages/index/index'}})">
					<image src="../../static/images/details-icon2.png"	 mode=""></image>
					<view>首页</view>
				</view>
				<view class="ite" @click="Router.switchTab({route:{path:'/pages/car/car'}})">
					<image src="../../static/images/details-icon1.png" mode=""></image>
					<view>购物车</view>
				</view>
				<button class="ite" open-type="share" >
					<image src="../../static/images/details-icon.png" mode=""></image>
					<view>分享</view>
				</button>
			</view>
			<view class="right flexRowBetween white fs13">
				<view class="item" style="background-color: #ff571c;">加入购物车</view>
				<view class="item pubBj" @click="goBuy()">立即下单</view>
			</view>
		</view>
		
		<!-- 10:00开抢 -->
		<view class="xqbotomBar" v-if="!mainData.isStart">
			<view class="left">
				<view class="ite" @click="Router.switchTab({route:{path:'/pages/index/index'}})">
					<image src="../../static/images/details-icon2.png" mode=""></image>
					<view>首页</view>
				</view>
				<view class="ite">
					<image src="../../static/images/details-icon1.png" mode=""></image>
					<view>购物车</view>
				</view>
				<button class="ite">
					<image src="../../static/images/details-icon.png" mode=""></image>
					<view>分享</view>
				</button>
			</view>
			<view class="right flexRowBetween white fs13">
				<view class="item" style="background-color: #ff571c;" @click="addCar">加入购物车</view>
				<view class="item pubBj">{{Utils.timeto(mainData.start_time,'h')}}点开抢</view>
			</view>
		</view>
		
		<!-- 已售空按钮 -->
		<view class="xqbotomBar flexRowBetween pdtb5" style="height: 110rpx" v-if="mainData.stock==0">
			<view class="left" style="width: 20%;">
				<view class="ite" style="width: 100%;" @click="Router.switchTab({route:{path:'/pages/index/index'}})">
					<image src="../../static/images/details-icon2.png" mode=""></image>
					<view>首页</view>
				</view>
			</view>
			<view class="fs15 pdtb10 center mgr15"  style="width: 75%;">
				<view class="f5bj fs14" style="line-height: 70rpx;border-radius: 40rpx;background-color: #ececec;">已售空</view>
			</view>
		</view>
		
		
		
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				messageData:[{},{},{},{}],
				mainData:{}
			}
		},
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		onShow() {
			const self = this;
			self.orderList = [];
			uni.removeStorageSync('payPro');
		},
		
		onShareAppMessage(ops) {
			console.log(ops)
			const self = this;
			if (ops.from === 'button') {
				return {
					title: self.mainData.title,
					path: '/pages/productDetail/productDetail?id='+self.mainData.id, //点击分享的图片进到哪一个页面
					imageUrl:self.mainData.mainImg[0].url?self.mainData.mainImg[0].url:'',
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
					title: self.mainData.title,
					path: '/pages/productDetail/productDetail?id='+self.mainData.id, //点击分享的图片进到哪一个页面
					imageUrl:self.mainData.mainImg[0].url?self.mainData.mainImg[0].url:'',
					success: function(res) {
						// 转发成功
						console.log("转发成功:" + JSON.stringify(res));
					},
					fail: function(res) {
						// 转发失败
						console.log("转发失败:" + JSON.stringify(res));
					}
				}
				console.log(ops.target)
			}
		},
		
		methods: {
			
			addCar(){
				const self = this;
				var array = self.$Utils.getStorageArray('cartData');
				for (var i = 0; i < array.length; i++) {
					if(array[i].id == self.id){
						var target = array[i]
					}
				}
				if(target){
					target.count  = target.count + 1
				}else{
					var target = self.mainData;
					target.count = 0;
					target.isCar = false;
					target.isSelect = false;
				}
				self.$Utils.setStorageArray('cartData', target, 'id', 999);
			},
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					id: self.id
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;"`);
						if(Date.parse(new Date())>parseInt(self.mainData.start_time)){
							self.mainData.isStart = true
						}else{
							self.mainData.isStart = false
						}
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},
			
			
			goBuy(){
				const self = this;
				uni.setStorageSync('canClick',false);
				self.orderList.push(
					{product_id:self.mainData.id,count:1,
					type:1,product:self.mainData},
				);
				uni.setStorageSync('payPro', self.orderList);
				self.Router.navigateTo({route:{path:'/pages/orderConfim/orderConfim'}})
				uni.setStorageSync('canClick',true);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/detail.css";
	page{padding-bottom: 140rpx;}
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
