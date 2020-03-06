<template>
	<view>
		
		<view class="detailxqBan">
			<image src="../../static/images/details-img.png" mode=""></image>
		</view>
		<view class="mglr4 pdb15 detailHead">
			<view class="tit borderB1">智利车厘子 约200g/份 正负20g</view>
			<view class="data flex">
				<view class="mgr20">预售时间：12月12日</view>
				<view>提货时间：12月21日</view>
			</view>
			<view class="fs13 color6">今日已售<span class="pubcolor">1232</span>份/限量5000份</view>
			<view class="pdt15">
				<view class="price fs18 ftw">18.8</view>
			</view>
		</view>
		<view class="f5H10"></view>
		<view class="mglr4 pdt20 pdb10 xqInfor">
			<view class="fs15 ftw mgb10">商品规格描述</view>
			<view class="cont">
				<view>回家路上隔离规划分开读后感日光华润的规划干活今飞凯达领放部分的需</view>
				<view>凤凰健康的时令水果认识人股份看过来工会福利卡挂号费进度款改好了肯定是个赶快来送过</view>
				<view class="">
					<image class="w" src="../../static/images/details-img1.png" mode="widthFix"></image>
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
		<view class="xqbotomBar">
			<view class="left">
				<view class="ite" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
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
				<view class="item" style="background-color: #ff571c;">加入购物车</view>
				<view class="item pubBj" @click="Router.redirectTo({route:{path:'/pages/orderConfim/orderConfim'}})">立即下单</view>
			</view>
		</view>
		
		<!-- 10:00开抢 -->
		<view class="xqbotomBar" style="bottom: 120rpx;">
			<view class="left">
				<view class="ite" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
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
				<view class="item" style="background-color: #ff571c;">加入购物车</view>
				<view class="item pubBj">10点开抢</view>
			</view>
		</view>
		
		<!-- 已售空按钮 -->
		<view class="xqbotomBar flexRowBetween pdtb5" style="height: 110rpx; bottom: 230rpx;">
			<view class="left" style="width: 20%;">
				<view class="ite" style="width: 100%;" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
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
				is_show: false,
				wx_info:{},
				is_show:false,
				messageData:[{},{},{},{}]
			}
		},
		onLoad() {
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
	@import "../../assets/style/detail.css";
	page{padding-bottom: 140rpx;}
	
</style>
