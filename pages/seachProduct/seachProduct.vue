<template>
	<view>
		<!-- 搜索 -->
		<view class="seachbox flexRowBetween whiteBj"  style="padding-bottom: 30rpx;"> 
			<view class="flex rr" style="width: 85%;">
				<button class="seachBtn" type="button"></button>
				<view class="input">
					<input type="text" name="" value="" placeholder="活动" placeholder-class="placeholder" />
				</view>
				<view class="delt flex"><text>×</text></view>
			</view>
			<view class="fs15 pubColor">搜索</view>
		</view>
		<view class="mglr4 proList" >
			<view class="item" v-for="(item,index) in proListData" :key="index">
				<view>
					<view class="center fs13 color6 mgb10">本商品由圣灵商行专供</view>
					<view class="pic">
						<image src="../../static/images/home-banner.png" mode=""></image>
					</view>
					<view class="tit borderB1">智利车厘子 约200g/份 正负20g</view>
					<view class="data flex">
						<view class="mgr20">预售时间：12月12日</view>
						<view>提货时间：12月21日</view>
					</view>
				</view>
				<view class="sellNum">今日已售<span class="pubcolor">1232</span>份/限量5000份</view>
				<view class="flexRowBetween B-price">
					<view class="price fs18 ftw">18.8</view>
					<view class="robBtn addCar">加入购物车</view>
				</view>
			</view>
		</view>
		
		<!-- 无数据 -->
		<view class="noDataBox">
			<image src="../../static/images/nodata.png" mode=""></image>
		</view>
		
		<view class="RFixIcon">
			<image src="../../static/images/search-icon.png" mode=""></image>
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
				proListData:[{},{}],
				mainData:[]
			}
		},
		
		onLoad(options) {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
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
	@import "../../assets/style/seach.css";
	@import "../../assets/style/proList.css";
	
	page{padding-bottom: 40rpx;background: #F5F5F5;}
	
</style>

