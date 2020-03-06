<template>
	<view>
		<view class="mglr4">
			<view class="flexEnd pdtb15">
				<view v-show="!is_bayBtn" @click="edit()">编辑</view>
				<view v-show="is_bayBtn" @click="edit()">完成</view>
			</view>
			<view class="proRow">
				<view class="item pr" v-for="(item,index) in mainData" :key="index">
					<view class="seltIcon" @click="currChange(index)"><image :src="curr==index?'../../static/images/shopping-icon.png':'../../static/images/shopping-icon1.png'" mode=""></image></view>
					<view class="flexRowBetween">
						<view class="pic">
							<image src="../../static/images/shopping-img.png" mode=""></image>
						</view>
						<view class="infor">
							<view class="avoidOverflow fs14">智利车厘子 约200g/份 正负20g</view>
							<view class="pubColor fs12">{{item.tag}}</view>
							<view class="B-price flexRowBetween">
								<view class="price fs16">18.8</view>
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
		<view class="noDataBox">
			<image src="../../static/images/nodata.png" mode=""></image>
		</view>
		
		<view class="xqbotomBar pdlr4" style="bottom: 110rpx;border-bottom: 1px solid #eee;">
			<view class="flex">
				<image class="seltIcon mgr10" src="../../static/images/shopping-icon1.png" mode=""></image>全选
			</view>
			<view class="flexEnd">
				
				<view class="flexEnd" v-show="!is_bayBtn">
					<view class="flex mgr15 fs12">合计：<span class="price fs16 ftw">18.8</span></view>
					<view class="radiuBtn pubBj white fs15" @click="Router.navigateTo({route:{path:'/pages/orderConfim/orderConfim'}})">结算</view>
				</view>
				<view class="flexEnd" v-show="is_bayBtn">
					<view class="flex mgr15 fs12">合计：<span class="price fs16 ftw">0</span></view>
					<view class="radiuBtn pubColor fs15" @click="popupShow">删除(0)</view>
				</view>
			</view>
		</view>
		
		<view class="black-bj" v-show="is_show"></view>
		<view class="popupShow center whiteBj radius10 pdt20" v-show="is_popupShow">
			<view class="tip-title mgb10 fs15">提示</view>
			<view class="fs13 color6 pdb20 borderB1">确定删除此商品吗？</view>
			<view class="flex tip-button">
				<view class="item"  @click="popupShow">取消</view>
				<view class="item pubColor">确定</view>
			</view>
		</view>
		
		<!--底部tab键-->
		<view class="navbar">
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
		</view>
		<!--底部tab键 end-->
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				is_show: false,
				wx_info:{},
				is_show:false,
				curr:0,
				mainData:[
					{count:1,tag:''},
					{count:1,tag:'10点开始购买'}
				],
				is_bayBtn:false,
				is_popupShow:false
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			counter(index,type) {
				const self = this;
				if (type == '+') {
					self.mainData[index].count++;
				} else {
					if (self.mainData[index].count > 1) {
						self.mainData[index].count--;
					}
				};
				self.$Utils.setStorageArray('cartData', self.mainData[index], 'id', 999);
				
				self.countTotalPrice();
			},
			currChange(index){
				const self = this;
				self.curr = index
			},
			edit(){
				const self = this;
				self.is_bayBtn = !self.is_bayBtn
			},
			popupShow(){
				const self=this;
				self.is_popupShow = !self.is_popupShow;
				self.is_show = !self.is_show;
				
			},
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				self.$apis.orderGet(postData, callback);
			}
		}
	};
</script>

<style>
	@import "../../assets/style/navbar.css";
	@import "../../assets/style/detail.css";
	@import "../../assets/style/myOrder.css";
	@import "../../assets/style/numBox.css";
	
	page{padding-bottom: 140rpx;background-color: #F5F5F5;}	
	
	.proRow .item{position: relative;padding-left: 80rpx;}
	.proRow .item .pic{width: 180rpx;height: 180rpx;}
	.proRow .item .infor{height: 180rpx;}
	.proRow .item .seltIcon{position: absolute;top: 50%;left: 20rpx;transform: translateY(-50%);}
	.seltIcon{width: 36rpx;height:36rpx;display: block;}
	.seltIcon image{width: 100%;height: 100%;}
	
	/* 删除弹框 */
	.popupShow{ width: 80%;position: fixed; top: 50%;left: 50%;transform: translate(-50%,-50%); z-index: 50;box-sizing: border-box;}
	.tip-button .item{width: 50%;box-sizing: border-box; line-height: 100rpx;}
	.tip-button .item:first-child{border-right:1px solid #eee;}
</style>
