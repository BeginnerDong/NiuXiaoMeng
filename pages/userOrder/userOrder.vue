<template>
	<view>
		
		<view class="orderNav flex fs14 whiteBj color6 boxShaow">
			<view class="tt" :class="current==1?'on':''" @click="change('1')">全部</view>
			<view class="tt" :class="current==2?'on':''" @click="change('2')">未付款</view>
			<view class="tt" :class="current==3?'on':''" @click="change('3')">待提货</view>
			<view class="tt" :class="current==4?'on':''" @click="change('4')">已提货</view>
			<view class="tt" :class="current==5?'on':''" @click="change('5')">已评价</view>
		</view>
		<view class="pdtb20"></view>
		<view class="mglr4">
			<view class="myOrderList fs12">
				<view class="item">
					<view @click="Router.navigateTo({route:{path:'/pages/orderConfim/orderConfim'}})">
						<view class="flexRowBetween stateLine pdtb15 borderB1">
							<view>订单编号：15558755542525</view>
							<view class="pubColor flexEnd">待付款</view>
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
					<view class="underBtn flexEnd">
						<view class="Bbtn" @click="Router.navigateTo({route:{path:'/pages/orderConfim/orderConfim'}})">去付款</view>
					</view>
				</view>
				<view class="item">
					<view  @click="Router.navigateTo({route:{path:'/pages/userOrder-detail/userOrder-detail'}})">
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
					<view>
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
					<view class="underBtn flexEnd">
						<view class="Bbtn" @click="Router.navigateTo({route:{path:'/pages/userOrder-pingjia/userOrder-pingjia'}})">去评价</view>
					</view>
				</view>
				<view class="item">
					<view  @click="Router.navigateTo({route:{path:'/pages/userOrder-detail/userOrder-detail'}})">
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
					<view class="underBtn flexEnd">
						<view class="Bbtn" @click="Router.navigateTo({route:{path:'/pages/userOrder-pingjiaList/userOrder-pingjiaList'}})">去评价</view>
					</view>
				</view>
				<view class="item">
					<view  @click="Router.navigateTo({route:{path:'/pages/userOrder-detail/userOrder-detail'}})">
						<view class="flexRowBetween stateLine pdtb15 borderB1">
							<view>订单编号：15558755542525</view>
							<view class="pubColor flexEnd">已评价</view>
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
					<view class="underBtn flexEnd">
						<view class="Bbtn gryBtn" @click="Router.navigateTo({route:{path:'/pages/userOrder-pingjiaok/userOrder-pingjiaok'}})">查看评价</view>
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
				score:'',
				wx_info:{},
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
		}
	};
</script>

<style>
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/myOrder.css";
	page{background: #F5F5F5;padding-bottom: 140rpx;}
	.orderNav{position: fixed;left: 0;top: 88rpx;right: 0; z-index: 5;}
	
</style>
