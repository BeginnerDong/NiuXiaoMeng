<template>
	<view>
		<view class="mglr4 mgt15">
			<view class=" radius10 fs16 whiteBj pdtb30 flexCenter mgb15" style="height: 100rpx;" @click="Router.navigateTo({route:{path:'/pages/zitiAddress/zitiAddress'}})"><span class="mgr5 fs20">+</span>自提门店</view>
			
			<view class="pdlr4 radius10 whiteBj pdt10 pdb10 flexCenter mgb15 flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/zitiAddress/zitiAddress'}})">
				<view style="width: 80%;">
					<view class="fs14 pdb5">门店名称门店名称</view>
					<view class="color6 fs13">陕西省西安市高新区大都汇座</view>
				</view>
				<view class="flexEnd" style="width: 20%;">
					<image class="arrowR" src="../../static/images/about-icon.png" mode=""></image>
				</view>
			</view>
			
			<view class="proRow">
				<view class="item">
					<view class="flexRowBetween">
						<view class="pic">
							<image src="../../static/images/shopping-img.png" mode=""></image>
						</view>
						<view class="infor">
							<view class="avoidOverflow fs14">智利车厘子 约200g/份 正负20g</view>
							<view class="B-price flexRowBetween">
								<view class="price fs16">18.8</view>
								<view class="fs13">×1</view>
							</view>
						</view>
					</view>
				</view>
			</view>	
			
			<view class="editLine whiteBj pdlr4 radius10 oh">
				<view class="item flexRowBetween">
					<view class="ll">收货人姓名</view>
					<view class="rr"><input type="text" placeholder="请输入" placeholder-style="placeholder"></view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll">电话</view>
					<view class="rr"><input type="text" placeholder="请输入" placeholder-style="placeholder"></view>
				</view>
			</view>
			
		</view>
		
		<view class="xqbotomBar">
			<view class="flex mgl15 fs13">总计：<view class="price fs16 ftw">56</view></view>
			<view class="payBtn fs15" @click="PayShow">提交订单</view>
		</view>
		
		<view class="black-bj" v-show="is_show"></view>
		<view class="PayShow center" v-show="is_PayShow">
			<view class="data fs12">12月21日 16:00提货</view>
			<view class="pubColor fs13 mgb15">此商品需要您到店自提，请仔细确认地址</view>
			<view class="f5bj radius10 mgb15 pdlr4 pdt15 pdb15">
				<view class="fs14 mgb5">自提门店：门店名称门店名称</view>
				<view class="fs13 color6">地址：陕西省西安市高新区大都荟A座</view>
			</view>
			<view class="seltBtn flexRowBetween fs13 color6" style="width:75%;margin:0 auto ;">
				<view class="btn" @click="PayShow">取消付款</view>
				<view class="btn on" @click="Router.navigateTo({route:{path:'/pages/onlinePay/onlinePay'}})">确认付款</view>
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
				is_show:false,
				is_PayShow:false
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			PayShow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_PayShow = !self.is_PayShow;
			}
		}
	};
</script>

<style>
	@import "../../assets/style/myOrder.css";
	@import "../../assets/style/detail.css";
	@import "../../assets/style/editInfor.css";
	page{background: #F5F5F5;padding-bottom: 140rpx;}
	.editLine .item:last-child{border-bottom: 0;}
	.editLine .item .rr input{text-align: right;}
	
	.PayShow{background-color: #fff;border-radius: 10rpx 10rpx 0 0;position: fixed;left: 0;right: 0;bottom: 0;z-index: 99;box-sizing: border-box;padding: 80rpx 4%;}
	.PayShow .data{width: 300rpx;height: 80rpx;line-height: 80rpx;background-color: #E1211C;color: #fff;padding: 0 30rpx;box-sizing: border-box;position: absolute;top: -30rpx;left: 50%;transform: translateX(-50%);border-radius: 10rpx;}
	
	.seltBtn .btn{width: 200rpx;height: 60rpx;line-height: 60rpx;border-radius:30rpx;border:1px solid #ccc;}
	.seltBtn .btn.on{border-color: #E1211C;color: #E1211C;}
	
</style>
