<template>
	<view>
		<view class="mglr4">
			<view class="flexEnd pdtb15">
				<view v-show="!is_bayBtn" @click="edit()">编辑</view>
				<view v-show="is_bayBtn" @click="edit()">完成</view>
			</view>
			<view class="proRow">
				<view class="item pr" v-for="(item,index) in mainData" :key="index">
					<view class="seltIcon" @click="currChange(index)">
						<image src="../../static/images/shopping-icon.png" v-if="item.isSelect" @click="choose(index)"></image>
						<image src="../../static/images/shopping-icon1.png" v-if="!item.isSelect" @click="choose(index)"></image>
					</view>
					<view class="flexRowBetween">
						<view class="pic">
							<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''"></image>
						</view>
						<view class="infor">
							<view class="avoidOverflow fs14">{{item.title}}</view>
							<view class="pubColor fs12" v-if="!item.isStart">{{Utils.timeto(item.start_time,'h')}}点开始购买</view>
							<view class="B-price flexRowBetween">
								<view class="price fs16">{{item.price}}</view>
								<view class="flexEnd">
									<view class="numBox flex">
										<view class="btn" @click="counter(index,'-')">-</view>
										<view class="num">{{item.count}}</view>
										<view class="btn add" @click="counter(index,'+')">+</view>
									</view>
								</view>
							</view>
						</view>
					</view>
				</view>
			</view>
		</view>

		<!-- 无数据 -->
		<view class="noDataBox" v-if="mainData.length==0">
			<image src="../../static/images/nodata.png" mode=""></image>
		</view>

		<view class="xqbotomBar pdlr4" style="border-bottom: 1px solid #eee;">
			<view class="flex">
				<image class="seltIcon mgr10" :src="isChooseAll?'../../static/images/shopping-icon.png':'../../static/images/shopping-icon1.png'" mode=""></image>全选
			</view>
			<view class="flexEnd">

				<view class="flexEnd" v-show="!is_bayBtn">
					<view class="flex mgr15 fs12">合计：<span class="price fs16 ftw">{{totalPrice}}</span></view>
					<view class="radiuBtn pubBj white fs15" @click="pay">结算</view>
				</view>
				<view class="flexEnd" v-show="is_bayBtn">
					<view class="flex mgr15 fs12">合计：<span class="price fs16 ftw">{{totalPrice}}</span></view>
					<view class="radiuBtn pubColor fs15" @click="popupShow">删除</view>
				</view>
			</view>
		</view>

		<view class="black-bj" v-show="is_show"></view>
		<view class="popupShow center whiteBj radius10 pdt20" v-show="is_popupShow">
			<view class="tip-title mgb10 fs15">提示</view>
			<view class="fs13 color6 pdb20 borderB1">确定删除此商品吗？</view>
			<view class="flex tip-button">
				<view class="item" @click="popupShow">取消</view>
				<view class="item pubColor" @click="deleteChoose">确定</view>
			</view>
		</view>

		<!--底部tab键-->
		<!-- <view class="navbar">
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar1.png" />
				</view>
				<view class="text">首页</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/car/car'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar2-a.png" />
				</view>
				<view class="text this-text">购物车</view>
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
				Router: this.$Router,
				is_show: false,
				Utils:this.$Utils,
				wx_info: {},
				curr: 0,
				mainData: [
				],
				is_bayBtn: false,
				is_popupShow: false,
				allCount:0,
				totalPrice:0,
				isChooseAll:true
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		
		onShow() {
			const self = this;
			self.mainData = self.$Utils.getStorageArray('cartData');
			for (var i = 0; i < self.mainData.length; i++) {
				if(Date.parse(new Date())>parseInt(self.mainData[i].start_time)){
					self.mainData[i].isStart = true
				}else{
					self.mainData[i].isStart = false
				}
			};
			
			self.checkChooseAll();
			self.countTotalPrice();
		},
		
		methods: {
			
			currChange(index) {
				const self = this;
				self.curr = index
			},
			
			edit() {
				const self = this;
				self.is_bayBtn = !self.is_bayBtn
			},
			
			popupShow() {
				const self = this;
				self.is_popupShow = !self.is_popupShow;
				self.is_show = !self.is_show;
			},

			checkChooseAll() {
				const self = this;
				var isChooseAll = true;
				for (var i = 0; i < self.mainData.length; i++) {
					if (!self.mainData[i].isSelect) {
						isChooseAll = false;
					};
				};
				self.isChooseAll = isChooseAll;
			},

			chooseAll() {
				const self = this;
				self.isChooseAll = !self.isChooseAll;
				for (var i = 0; i < self.mainData.length; i++) {
					self.mainData[i].isSelect = self.isChooseAll;
					self.$Utils.setStorageArray('cartData', self.mainData[i], 'id', 999);
				};
				self.countTotalPrice();
			},

				deleteChoose() {
					const self = this;
					for (var i = 0; i < self.mainData.length; i++) {
						if (self.mainData[i].isSelect) {
							self.$Utils.delStorageArray('cartData', self.mainData[i], 'id');
						}
					};
					self.mainData = self.$Utils.getStorageArray('cartData');
					console.log('2323',self.mainData)
					self.checkChooseAll();
					self.countTotalPrice();
					self.checkStatus();
					self.popupShow()
				},

			choose(index) {
				const self = this;

				if (self.mainData[index].isSelect) {
					self.mainData[index].isSelect = false;
				} else {
					self.mainData[index].isSelect = true;
				};
				self.$Utils.setStorageArray('cartData', self.mainData[index], 'id', 999);
				
				self.checkChooseAll();
				self.countTotalPrice();
			},
			
			checkStatus(){
				const self = this;
				self.allCount = 0;
				for (var i = 0; i < self.mainData.length; i++) {
					if(self.mainData[i].count>0){
						self.mainData[i].isCar = true;
						//self.mainData[i].isSelect = true;
						self.allCount += self.mainData[i].count;
						self.$Utils.setStorageArray('cartData', self.mainData[i], 'id', 999);
						self.$Utils.setStorageArray('indexData', self.mainData[i], 'id', 999);
					
						
					}
				};
				if(self.mainData.length==0){
					var indexData = self.$Utils.getStorageArray('indexData');
					for (var i = 0; i < indexData.length; i++) {
						indexData[i].count = 0
						indexData[i].isCar = false;
						indexData[i].isSelect = false;
						
						self.$Utils.setStorageArray('indexData', indexData[i], 'id', 999);
					}
				}
				uni.setTabBarBadge({
				  index: 1,
				  text: self.allCount.toString()
				})
			},

			countTotalPrice() {
				const self = this;
				self.totalPrice = 0;

				for (var i = 0; i < self.mainData.length; i++) {
					if (self.mainData[i].isSelect) {
						self.totalPrice += self.mainData[i].price * self.mainData[i].count;
					};
				};
			},



			pay(e) {
				const self = this;
				const orderList = [
				];
				for (var i = 0; i < self.mainData.length; i++) {
					if (self.mainData[i].isSelect) {
						orderList.push({
							product_id: self.mainData[i].id,
							count: self.mainData[i].count,
							product: self.mainData[i]
						}, );
					};
					if(!self.mainData[i].isStart){
						self.$Utils.showToast('所选商品未开售', 'none', 1000);
						return;
					}
				};
				if (orderList.length == 0) {
					self.$Utils.showToast('未选择商品', 'none', 1000);
					return;
				};
				uni.setStorageSync('payPro', orderList);
				self.$Router.navigateTo({
					route: {
						path: '/pages/orderConfim/orderConfim'
					}
				})


			},


			counter(index, type) {
				const self = this;

				if (type == '+') {
					self.mainData[index].count++;
				} else {
					if (self.mainData[index].count > 1) {
						self.mainData[index].count--;
					}
				};
				self.checkStatus()
				
				self.countTotalPrice();
			},
		}
	};
</script>

<style>
	@import "../../assets/style/navbar.css";
	@import "../../assets/style/detail.css";
	@import "../../assets/style/myOrder.css";
	@import "../../assets/style/numBox.css";

	page {
		background-color: #F5F5F5;
	}

	.proRow .item {
		position: relative;
		padding-left: 80rpx;
	}

	.proRow .item .pic {
		width: 180rpx;
		height: 180rpx;
	}

	.proRow .item .infor {
		height: 180rpx;
	}

	.proRow .item .seltIcon {
		position: absolute;
		top: 50%;
		left: 20rpx;
		transform: translateY(-50%);
	}

	.seltIcon {
		width: 36rpx;
		height: 36rpx;
		display: block;
	}

	.seltIcon image {
		width: 100%;
		height: 100%;
	}

	/* 删除弹框 */
	.popupShow {
		width: 80%;
		position: fixed;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		z-index: 50;
		box-sizing: border-box;
	}

	.tip-button .item {
		width: 50%;
		box-sizing: border-box;
		line-height: 100rpx;
	}

	.tip-button .item:first-child {
		border-right: 1px solid #eee;
	}
</style>
