<template>
	<view>
		<view class="seachbox flexRowBetween whiteBj borderB1"  style="padding-bottom: 30rpx;"> 
			<view class="flex rr" style="width: 85%;">
				<button class="seachBtn" type="button"></button>
				<view class="input">
					<input type="text" focus="true"  v-model="keywords" placeholder="搜索单号、手机号、收货人" placeholder-class="placeholder" />
				</view>
				<view class="delt flex"></view>
			</view>
			<view class="fs15 pubColor" @click="Router.redirectTo({route:{path:'/pages/business-SeachOrder/business-SeachOrder'}})">搜索</view>
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
				mainData:[],
				is_popupShow:false,
				keywords:''
			}
		},
		
		onLoad(options) {
			const self = this;
		},
		
		
		methods: {
			
			
			
			
			
			search(){
				const self = this;
				if(self.keywords!=''){
					self.Router.navigateTo({route:{path:'/pages/business-SeachOrder/business-SeachOrder?keywords='+self.keywords}});
					self.keywords = '';
				}else{
					return
				}
			},
			
			
			
		}
	};
</script>

<style>
	@import "../../assets/style/seach.css";
	
	page{padding-bottom: 110rpx;background-color: #F5F5F5;}
	
</style>

