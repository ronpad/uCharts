<template>
	<view class="about">
		<!-- #ifdef MP-WEIXIN -->
			<ad unit-id="adunit-908b0a16e90e2a5f" ad-type="grid" grid-count="8" ad-theme="white"></ad>
			<!-- #endif -->
		<view class="content">
      <!--#ifdef H5-->
			<view class="qrcode">
				<image src="../../static/images/qrcode.png" @longtap="save"></image>
				<text class="tip">扫码体验uCharts</text>
			</view>
      <!--#endif-->
			
			<view class="desc">
				<text class="code">uCharts</text>
				是一个基于 <text class="code">uni-app</text> 开发跨平台应用的<text class="code">高性能图表工具</text>。
			</view>
			<view class="source">
				<view class="title">本示例源码获取方式：</view>
				<view class="source-list">
					<view class="source-cell">
						<text space="nbsp">1. </text>
						<text>下载 HBuilderX，新建 uni-app 项目时选择 <text class="code">uCharts</text> 模板。</text>
					</view>
					<view class="source-cell">
						<text space="nbsp">2. </text><text @click="openLink" class="link">{{sourceLink}}</text>
					</view>
				</view>
			</view>
			<!-- #ifdef APP-PLUS -->
			<button type="primary" @click="share">分享</button>
			<!-- #endif -->
		</view>
		<!-- #ifdef APP-PLUS -->
		<view class="version">
			当前版本：{{version}}
		</view>
		<!-- #endif -->
	</view>
</template>

<script>
	export default {
		data() {
			return {
				providerList: [],
				version: '',
				sourceLink: 'https://www.ucharts.cn'
			}
		},
		onLoad() {
			// #ifdef APP-PLUS
			this.version = plus.runtime.version;
			uni.getProvider({
				service: 'share',
				success: (result) => {
					const data = [];
					for (let i = 0; i < result.provider.length; i++) {
						switch (result.provider[i]) {
							case 'weixin':
								data.push({
									name: '分享到微信好友',
									id: 'weixin'
								});
								data.push({
									name: '分享到微信朋友圈',
									id: 'weixin',
									type: 'WXSenceTimeline'
								});
								break;
							case 'qq':
								data.push({
									name: '分享到QQ',
									id: 'qq'
								});
								break;
							default:
								break;
						}
					}
					this.providerList = data;
				},
				fail: (error) => {
					console.log('获取登录通道失败' + JSON.stringify(error));
				}
			});
		// #endif
		},
		onShareAppMessage(){
				//#ifdef MP-QQ
				qq.showShareMenu({showShareItems: ['qq', 'qzone', 'wechatFriends', 'wechatMoment']})
				//#endif
		},
		methods: {
			// #ifdef APP-PLUS
			save() {
				uni.showActionSheet({
					itemList: ['保存图片到相册'],
					success: () => {
						plus.gallery.save('../../static/images/qrcode.png', function() {
							uni.showToast({
								title: '保存成功',
								icon: 'none'
							});
						}, function() {
							uni.showToast({
								title: '保存失败，请重试！',
								icon: 'none'
							});
						});
					}
				});
			},
			share(e) {
				if (this.providerList.length === 0) {
					uni.showModal({
						title: '当前环境无分享渠道!',
						showCancel: false
					});
					return;
				}
				let itemList = this.providerList.map(function(value) {
					return value.name;
				})
				uni.showActionSheet({
					itemList: itemList,
					success: (res) => {
						uni.share({
							provider: this.providerList[res.tapIndex].id,
							scene: this.providerList[res.tapIndex].type && this.providerList[res.tapIndex].type === 'WXSenceTimeline' ?
								'WXSenceTimeline' : "WXSceneSession",
							type: 0,
							title: '欢迎体验uCharts',
							summary: 'uCharts',
							imageUrl: 'https://img.cdn.aliyun.dcloud.net.cn/stream/plugin_screens/7908a740-5488-11e9-b307-d310fdb71328_0.png?v=1558666216',
							href: "https://www.ucharts.cn",
							success: (res) => {
								console.log("success:" + JSON.stringify(res));
							},
							fail: (e) => {
								uni.showModal({
									content: e.errMsg,
									showCancel: false
								})
							}
						});
					}
				})
			},
			// #endif
			openLink() {
				// #ifdef APP-PLUS
				plus.runtime.openURL(this.sourceLink);
				// #endif
				// #ifdef H5
				window.open(this.sourceLink);
				// #endif
			}
		}
	}
</script>

<style>
	page,
	view {
		display: flex;
	}

	page {
		min-height: 100%;
		background-color: #f4f5f6;
	}

	image {
		width: 360upx;
		height: 360upx;
	}

	.about {
		flex-direction: column;
		flex: 1;
	}

	.content {
		flex: 1;
		padding: 30upx;
		flex-direction: column;
	}

	.qrcode {
		display: flex;
		align-items: center;
		flex-direction: column;
	}

	.qrcode .tip {
		margin-top: 20upx;
	}

	.desc {
		display: block;
	}

	.code {
		color: #e96900;
		background-color: #f4f5f6;
	}

	button {
		width: 100%;
		margin-top: 40upx;
	}

	.version {
		height: 80upx;
		line-height: 80upx;
		justify-content: center;
		color: #ccc;
	}

	.source {
		margin-top: 30upx;
		flex-direction: column;
	}

	.source-list {
		flex-direction: column;
	}

	.link {
		color: #007AFF;
	}
</style>
