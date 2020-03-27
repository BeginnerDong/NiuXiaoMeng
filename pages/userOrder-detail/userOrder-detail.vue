<template>
	<view>
		<view class="mglr4 mgt15">
			<view class="pdlr4 HeadState flexRowBetween radius10">
				<view class="flex pubColor fs14" v-if="mainData.transport_status==0&&mainData.isremark==0">
					<image style="width: 30rpx; height: 30rpx;margin-right: 10rpx;" src="../../static/images/order-details-icon.png"
					 mode=""></image>
					未提货
				</view>
				<view class="flex pubColor fs14" v-if="mainData.transport_status==2&&mainData.isremark==0">
					<image style="width: 30rpx; height: 30rpx;margin-right: 10rpx;" src="../../static/images/order-details-icon.png"
					 mode=""></image>
					已提货
				</view>
				<view class="flex pubColor fs14" v-if="mainData.transport_status==2&&mainData.isremark==1">
					<image style="width: 30rpx; height: 30rpx;margin-right: 10rpx;" src="../../static/images/order-details-icon.png"
					 mode=""></image>
					已评价
				</view>
				<view class="flexEnd">
					<view class="flexEnd">提货单号：<span class="pubColor">{{mainData.code}}</span></view>
					<view class="mgl15">
						<image class="samllEwm" @click="previewImg(0)" :src="mainData.qrcode" mode=""></image>
					</view>
				</view>
			</view>
			<view class="whiteBj radius10 pdlr4 mgt15">
				<view class="pdtb15 fs14 ">
					<view>收货人：{{mainData.name?mainData.name:''}} {{mainData.phone?mainData.phone:''}}</view>
					<view class="pdtb15 pubColor">自提点：{{mainData.shop&&mainData.shop.name?mainData.shop.name:''}}</view>
					<view class="color6">地址：{{mainData.shop&&mainData.shop.address?mainData.shop.address:''}}</view>
				</view>
			</view>

			<view class="proRow mgt15">
				<view class="item" v-for="(item,index) in mainData.child" :key="index">
					<view class="flexRowBetween">
						<view class="pic">
							<image :src="item.orderItem&&item.orderItem[0]&&item.orderItem[0].snap_product
							&&item.orderItem[0].snap_product.mainImg&&item.orderItem[0].snap_product.mainImg[0]?item.orderItem[0].snap_product.mainImg[0].url:''"
							 mode=""></image>
						</view>
						<view class="infor">
							<view class="avoidOverflow fs14">{{item.title}}</view>
							<view class="B-price flexRowBetween">
								<view class="price fs16">{{item.unit_price}}</view>
								<view class="fs13">×{{item.count}}</view>
							</view>
						</view>
					</view>
					<view class="flexEnd pdt15 fs13">共{{item.count}}件商品 实付金额:<span class="price fs15 ftw">{{item.price}}</span></view>
				</view>
			</view>

			<view class="whiteBj radius10 pdlr4 mgt15">
				<view class="pdtb15 fs13 color6">
					<view class="mgb10">订单编号：{{mainData.order_no?mainData.order_no:''}}</view>
					<view>下单时间：{{mainData.create_time?mainData.create_time:''}}</view>
				</view>
			</view>

			<view class="fs13 color6 mgt20">1.如果您购买的商品有任何问题，请直接购买的门店联系！100%收货保障！</view>
			<view class="fs13 color6 mgt10">2.如果您找不到购物的提货门店，请致电牛小萌庄园帮忙热线！12356235862</view>
			<view class="flexEnd center" style="margin-top: 100rpx;">
				<view class="B-btn" @click="orderUpdate" v-if="mainData.transport_status==2">删除</view>
				<view class="B-btn on mgl15" @click="Router.switchTab({route:{path:'/pages/index/index'}})">继续购物</view>
			</view>
		</view>

		<view class="black-bj" v-show="is_show"></view>
		<view class="popupShow center whiteBj radius10 pdt20" v-show="is_popupShow">
			<view class="tip-title mgb10 fs15">提示</view>
			<view class="fs13 color6 pdb20 borderB1">确定清空搜索记录吗？</view>
			<view class="flex tip-button">
				<view class="item" @click="popupShow">取消</view>
				<view class="item pubColor" @click="clearHistory()">确定</view>
			</view>
		</view>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router: this.$Router,
				mainData: {}
			}
		},

		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData'], self);
		},

		methods: {

			previewImg() {
				const self = this;
				var urls = [self.mainData.qrcode]
				uni.previewImage({
					urls: urls,
					longPressActions: {
						itemList: ['发送给朋友', '保存图片', '收藏'],
						success: function(data) {
							
						},
						fail: function(err) {
							console.log(err.errMsg);
						}
					}
				});
			},

			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					id: self.id,
				};
				postData.getAfter = {
					shop: {
						tableName: 'UserInfo',
						middleKey: 'shop_no',
						key: 'user_no',
						searchItem: {
							status: 1,
							user_type: 1
						},
						condition: '=',
						info: ['name', 'address']
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
					};
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},

			orderUpdate(id) {
				const self = this;
				uni.setStorageSync('canClick', false);
				uni.showModal({
					title: '提示',
					content: '确定要删除此订单吗',
					confirmColor: '#FF2121',
					success: function(res) {
						if (res.confirm) {
							const postData = {};
							postData.tokenFuncName = 'getProjectToken';
							postData.data = {
								status: -1
							};
							postData.searchItem = {
								id: self.id,
							};
							const callback = (data) => {
								uni.setStorageSync('canClick', true);
								if (data && data.solely_code == 100000) {
									self.$Utils.showToast('操作成功', 'none');

									setTimeout(function() {
										uni.navigateBack({
											delta: 1
										})
									}, 1000);
								} else {
									self.$Utils.showToast(data.msg, 'none')
								}
							};
							self.$apis.orderUpdate(postData, callback);
						} else if (res.cancel) {

						}
					}
				});

			},
		}
	};
</script>

<style>
	@import "../../assets/style/myOrder.css";

	page {
		background: #F5F5F5;
		padding-bottom: 60rpx;
	}

	.underBtn {
		border-top: 0;
		padding-bottom: 0;
	}

	.HeadState {
		height: 143rpx;
		box-sizing: border-box;
		padding: 14rpx 4% 0 4%;
		background: url(../../static/images/order details-img.png) no-repeat 0 0/100% auto;
	}

	.B-btn {
		width: 200rpx;
		height: 60rpx;
		line-height: 60rpx;
		border-radius: 30rpx;
		border: 1px solid #ccc;
	}

	.B-btn.on {
		border-color: #E1211C;
		color: #E1211C;
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

	.samllEwm {
		width: 78rpx;
		height: 78rpx;
	}
</style>
