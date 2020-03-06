<template>
	<view>
		
		<!-- 评价 -->
		<view class="pdlr4">
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
	page{padding-bottom: 60rpx;}
	
</style>
