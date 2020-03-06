<template>
	<view>
		<view class="mglr4 mgt15">
			<view class="proRow">
				<view class="item" v-for="(item,index) in mainData" :key="index">
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
					<view class="flexEnd pdt15 fs13">共1件商品 实付金额:<span class="price fs15 ftw">56</span></view>
					<view class="underBtn flexEnd">
						<view class="Bbtn" @click="Router.navigateTo({route:{path:'/pages/userOrder-pingjia/userOrder-pingjia'}})">去评价</view>
					</view>
				</view>
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
				mainData:[{},{},{}]
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
		}
	};
</script>

<style>
	@import "../../assets/style/myOrder.css";
	page{background: #F5F5F5;padding-bottom: 60rpx;}
	.underBtn{border-top:0;padding-bottom: 0;}
</style>
