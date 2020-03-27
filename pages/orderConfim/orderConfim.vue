<template>
	<view>
		<view class="mglr4 mgt15">
			
			<view class="pdlr4 radius10 whiteBj pdt10 pdb10 flexCenter mgb15 flexRowBetween" v-if="addressData.address"
			@click="Router.navigateTo({route:{path:'/pages/zitiAddress/zitiAddress'}})">
				<view style="width: 80%;">
					<view class="fs14 pdb5">{{addressData.name}}</view>
					<view class="color6 fs13">{{addressData.address}}</view>
				</view>
				<view class="flexEnd" style="width: 20%;">
					<image class="arrowR" src="../../static/images/about-icon.png" mode=""></image>
				</view>
			</view>
			
			<view class=" radius10 fs16 whiteBj pdtb30 flexCenter mgb15" style="height: 100rpx;" v-else
			@click="Router.navigateTo({route:{path:'/pages/zitiAddress/zitiAddress'}})"><span class="mgr5 fs20">+</span>自提门店</view>
			
			
			
			<view class="proRow">
				<view class="item" v-for="item in mainData" :key="index">  
					<view class="flexRowBetween">
						<view class="pic">
							<image :src="item.product&&item.product.mainImg&&item.product.mainImg[0]?item.product.mainImg[0].url:''" mode=""></image>
						</view>
						<view class="infor">
							<view class="avoidOverflow fs14">{{item.product&&item.product.title?item.product.title:''}}</view>
							<view class="B-price flexRowBetween">
								<view class="price fs16">{{item.product&&item.product.title?item.product.price:''}}</view>
								<view class="fs13">×{{item.count}}</view>
							</view>
						</view>
					</view>
				</view>
			</view>	
			
			<view class="editLine whiteBj pdlr4 radius10 oh">
				<view class="item flexRowBetween">
					<view class="ll">收货人姓名</view>
					<view class="rr"><input type="text" v-model="submitData.name" placeholder="请输入" placeholder-style="placeholder"></view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll">电话</view>
					<view class="rr"><input type="text" v-model="submitData.phone" placeholder="请输入" placeholder-style="placeholder"></view>
				</view>
			</view>
			
		</view>
		
		<view class="xqbotomBar">
			<view class="flex mgl15 fs13">总计：<view class="price fs16 ftw">{{totalPrice}}</view></view>
			<button class="payBtn fs15"   @click="showToast">提交订单</button>
		</view>
		
		<view class="black-bj" v-show="is_show"></view>
		<view class="PayShow center" v-show="is_PayShow">
			<view class="data fs12">{{Utils.timeto(mainData[0].product.end_time,'md')}} {{Utils.timeto(mainData[0].product.end_time,'hm')}}提货</view>
			<view class="pubColor fs13 mgb15">此商品需要您到店自提，请仔细确认地址</view>
			<view class="f5bj radius10 mgb15 pdlr4 pdt15 pdb15">
				<view class="fs14 mgb5">自提门店：{{addressData.name}}</view>
				<view class="fs13 color6">地址：{{addressData.address}}</view>
			</view>
			<view class="seltBtn flexRowBetween fs13 color6" style="width:75%;margin:0 auto ;">
				<view class="btn" @click="PayShow">取消付款</view>
				<button class="btn on" style="font-size: 24rpx;" open-type="getUserInfo"  @getuserinfo="Utils.stopMultiClick(submit)">确认付款</button>
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
				is_show:false,
				is_PayShow:false,
				mainData:[],
				addressData:{},
				pay:{},
				totalPrice:0,
				submitData:{
					name:'',
					phone:''
				}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.mainData = uni.getStorageSync('payPro');
			self.countTotalPrice()
		},
		
		onShow() {
			const self = this;
			if(uni.getStorageSync('choosedAddressData')){
				self.addressData = uni.getStorageSync('choosedAddressData')
			}else{
				self.getUserInfoData()
			};
		},
		
		methods: {
			
			showToast(){
				const self = this;
				if(JSON.stringify(self.addressData) == '{}'){
					uni.setStorageSync('canClick',true);
					self.$Utils.showToast('请选择自提地址','none')
					return
				};
				console.log('22',self.addressData)
				if (self.submitData.name == '') {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请输入收货人姓名', 'none')
					return
				};
				if (self.submitData.phone == '') {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请输入收货人电话', 'none')
					return
				};
				self.is_show = !self.is_show;
				self.is_PayShow = !self.is_PayShow;
			},
			
			submit(){
				const self = this;
				uni.setStorageSync('canClick', false);
				var orderList = []
				for (var i = 0; i < self.mainData.length; i++) {
					orderList.push({product_id:self.mainData[i].product_id,count:self.mainData[i].count,type:1})
				}
				const callback = (user, res) => {
					self.addOrder(orderList)
				};
				self.$Utils.getAuthSetting(callback);
			},
			
			addOrder(orderList) {
				const self = this;	
				
				/* if(self.orderId){
					self.goPay()
					return
				}; */
				const postData = {}; 
				postData.orderList = self.$Utils.cloneForm(orderList);
				postData.data = {
					shop_no:self.addressData.user_no,
					level:1,
					name:self.submitData.name,
					phone:self.submitData.phone
				};
				postData.parent = 1;
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					uni.setStorageSync('canClick', true);
					if (res && res.solely_code == 100000) {
						self.orderId = res.info.id;
						for (var i = 0; i < orderList.length; i++) {
							self.$Utils.delStorageArray('cartData', orderList[i], 'id');
						}
						self.goPay()
					} else {		
						uni.showToast({
							title: res.msg,
							duration: 2000
						});
					};		
				};
				self.$apis.addOrder(postData, callback);
			},
			
			goPay(order_id) {
				const self = this;	
				if(self.orderId){
					self.Router.redirectTo({route:{path:'/pages/onlinePay/onlinePay?id='+self.orderId}})
				}else{
					self.$Utils.showToast('订单错误', 'none')
				}
			},
			
			getUserInfoData() {
				const self = this;
				const postData = {
					searchItem:{thirdapp_id:2}
				};
				postData.tokenFuncName = 'getProjectToken'
				postData.searchItem.user_no = uni.getStorageSync('user_info').user_no;
				postData.getAfter = {
					shop:{
						tableName:'UserInfo',
						middleKey:'shop_no',
						key:'user_no',
						searchItem:{status:1},
						condition:'='
					}
				};
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.userInfoData = res.info.data[0];
						if(res.info.data[0].shop[0]){
							self.addressData = res.info.data[0].shop[0]
						}
						
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
					// self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			PayShow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_PayShow = !self.is_PayShow;
			},
			
			countTotalPrice() {
				const self = this;
				self.totalPrice = 0;
				for (var i = 0; i < self.mainData.length; i++) {
					self.totalPrice += self.mainData[i].product.price*self.mainData[i].count
				};
				self.totalPrice = parseFloat(self.totalPrice).toFixed(2)
				//console.log('wxPay',wxPay)
				if (self.totalPrice > 0) {
					self.pay.wxPay = {
						price: self.totalPrice,
					};
				} else {
					  delete self.pay.wxPay;	 
				};
				console.log(self.pay)
			},
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
