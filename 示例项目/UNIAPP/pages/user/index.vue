<template>
	<view class="center">
		<view class="logo" @click="goLogin" :hover-class="!login ? 'logo-hover' : ''">
			<image class="logo-img" :src="login ? uerInfo.avatarUrl :avatarUrl"></image>
			<view class="logo-title">
				<text class="uer-name">Hi，{{login ? uerInfo.name : '您未登录'}}</text>
				<text class="go-login navigat-arrow" v-if="!login">&#xe65e;</text>
			</view>
		</view>
		<view class="center-list">
			<view @click="gourl('account')">
				<view class="center-list-item border-bottom">
					<text class="list-icon">&#xe60f;</text>
					<text class="list-text">帐号管理</text>
					<text class="navigat-arrow">&#xe65e;</text>
				</view>
			</view>
			<view @click="gourl('message')">
				<view class="center-list-item">
					<text class="list-icon">&#xe639;</text>
					<text class="list-text">新消息通知</text>
					<text class="navigat-arrow">&#xe65e;</text>
				</view>
			</view>
		</view>
		<view class="center-list">
			<navigator url="feedback" hover-class="navigator-hover">
				<view class="center-list-item border-bottom">
					<text class="list-icon">&#xe60b;</text>
					<text class="list-text">问题反馈</text>
					<text class="navigat-arrow">&#xe65e;</text>
				</view>
			</navigator>
			<navigator url="service" hover-class="navigator-hover">
				<view class="center-list-item">
					<text class="list-icon">&#xe65f;</text>
					<text class="list-text">服务条款及协议</text>
					<text class="navigat-arrow">&#xe65e;</text>
				</view>
			</navigator>
		</view>
		<view class="center-list">
			<navigator url="about" hover-class="navigator-hover">
				<view class="center-list-item">
					<text class="list-icon">&#xe614;</text>
					<text class="list-text">关于应用</text>
					<text class="navigat-arrow">&#xe65e;</text>
				</view>
			</navigator>
		</view>
	</view>
</template>

<script>
	var interstitialAd = null;
	export default {
		data() {
			return {
				login: false,
				avatarUrl: "../../static/images/avatar.png",
				uerInfo: {}
			}
		},
		onLoad() {
			let userinfo = uni.getStorageSync('userinfo');
			if (userinfo) {
				this.login=true;
				this.uerInfo = userinfo;
			}
			if (wx.createInterstitialAd) {
			  interstitialAd = wx.createInterstitialAd({
			    adUnitId: 'adunit-035d0c6533e6f944'
			  })
			  interstitialAd.onLoad(() => {})
			  interstitialAd.onError((err) => {})
			  interstitialAd.onClose(() => {})
			}
		},
		onShareAppMessage(){
				//#ifdef MP-QQ
				qq.showShareMenu({showShareItems: ['qq', 'qzone', 'wechatFriends', 'wechatMoment']})
				//#endif
		},
		onShow() {
			if (interstitialAd) {
			  interstitialAd.show().catch((err) => {
			    console.error(err)
			  })
			}
		},
		methods: {
			goLogin() {
				if (!this.login) {
					uni.navigateTo({
						url: 'login'
					});
				}
			},
			gourl(url){
				if (this.login) {
					uni.navigateTo({
						url: url
					});
				}else{
					uni.navigateTo({
						url: 'login'
					});
				}
			}
		}
	}
</script>

<style>
	@font-face {
		font-family: texticons;
		font-weight: normal;
		font-style: normal;
		src: url('https://at.alicdn.com/t/font_984210_5cs13ndgqsn.ttf') format('truetype');
	}

	page,
	view {
		display: flex;
	}

	page {
		background-color: #f4f5f6;
	}

	.center {
		flex-direction: column;
	}

	.logo {
		width: 750upx;
		height: 240upx;
		padding: 20upx;
		box-sizing: border-box;
		background-color: #2fc25b;
		flex-direction: row;
		align-items: center;
	}

	.logo-hover {
		opacity: 0.8;
	}

	.logo-img {
		width: 150upx;
		height: 150upx;
		border-radius: 150upx;
	}

	.logo-title {
		height: 150upx;
		flex: 1;
		align-items: center;
		justify-content: space-between;
		flex-direction: row;
		margin-left: 20upx;
	}

	.uer-name {
		height: 60upx;
		line-height: 60upx;
		font-size: 38upx;
		color: #FFFFFF;
	}

	.go-login.navigat-arrow {
		font-size: 38upx;
		color: #FFFFFF;
	}

	.login-title {
		height: 150upx;
		align-items: self-start;
		justify-content: center;
		flex-direction: column;
		margin-left: 20upx;
	}

	.center-list {
		background-color: #FFFFFF;
		margin-top: 20upx;
		width: 750upx;
		flex-direction: column;
	}

	.center-list-item {
		height: 90upx;
		width: 750upx;
		box-sizing: border-box;
		flex-direction: row;
		padding: 0upx 20upx;
	}

	.border-bottom {
		border-bottom-width: 1upx;
		border-color: #c8c7cc;
		border-bottom-style: solid;
	}

	.list-icon {
		width: 40upx;
		height: 90upx;
		line-height: 90upx;
		font-size: 34upx;
		color: #2fc25b;
		text-align: center;
		font-family: texticons;
		margin-right: 20upx;
	}

	.list-text {
		height: 90upx;
		line-height: 90upx;
		font-size: 34upx;
		color: #555;
		flex: 1;
		text-align: left;
	}

	.navigat-arrow {
		height: 90upx;
		width: 40upx;
		line-height: 90upx;
		font-size: 34upx;
		color: #555;
		text-align: right;
		font-family: texticons;
	}
</style>