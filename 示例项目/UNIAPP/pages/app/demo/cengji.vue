<template>
	<view class="qiun-columns">
		<view style="height: 400rpx;">
			<image src="../../../static/images/banner/banner2.png" style="width: 750rpx;height: 400rpx;"></image>
		</view>
		<view class="qiun-text-tips">Tips：下面是scroll-view，请滑动图表查看是否被上方图片覆盖。</view>
		<scroll-view scroll-y="true" style="height: 400rpx;" @scroll="scroll">
			
			<!--#ifdef H5 -->
			<view class="qiun-bg-white qiun-title-bar qiun-common-mt" >
				<view class="qiun-title-dot-light">页面地址</view>
			</view>
			<view class="qiun-bg-white qiun-padding">
					<text>pages/app/demo/cengji</text>
			</view>
			<!--#endif-->
			<view class="qiun-bg-white qiun-title-bar qiun-common-mt" >
				<view class="qiun-title-dot-light">基本柱状图</view>
			</view>
			<view class="qiun-charts" >
				<!-- 注意，这里用里@tap来触发事件 -->
				<canvas canvas-id="canvasColumn" class="charts" ></canvas>
			</view>
			<view style="height: 400rpx;">
				<image src="../../../static/images/banner/banner1.png" style="width: 750rpx;height: 400rpx;"></image>
			</view>
			
			<!--#ifdef H5 -->
			<view class="qiun-bg-white qiun-title-bar qiun-common-mt" >
				<view class="qiun-title-dot-light">标准数据格式</view>
			</view>
			<view class="qiun-bg-white qiun-padding">
					<textarea class="qiun-textarea" auto-height="true" maxlength="-1" v-model="textarea"/>
			</view>
			<view class="qiun-text-tips">Tips：修改后点击更新图表</view>
			<button class="qiun-button" @tap="changeData()">更新图表</button>
			<!--#endif-->
		</scroll-view>
		
	</view>
</template>

<script>
	import uCharts from '@/js_sdk/u-charts/u-charts.js';
	import  { isJSON } from '@/common/checker.js';
	var _self;
	var canvaColumn=null;
	export default {
		data() {
			return {
				cWidth:'',
				cHeight:'',
				pixelRatio:1,
				textarea:'',
				scrollTop: 0
			}
		},
		onShareAppMessage(){
				//#ifdef MP-QQ
				qq.showShareMenu({showShareItems: ['qq', 'qzone', 'wechatFriends', 'wechatMoment']})
				//#endif
		},
		onLoad() {
			_self = this;
			//#ifdef MP-ALIPAY
			uni.getSystemInfo({
				success: function (res) {
					if(res.pixelRatio>1){
						//正常这里给2就行，如果pixelRatio=3性能会降低一点
						//_self.pixelRatio =res.pixelRatio;
						_self.pixelRatio =2;
					}
				}
			});
			//#endif
			this.cWidth=uni.upx2px(750);
			this.cHeight=uni.upx2px(500);
			this.getServerData();
		},
		methods: {
			getServerData(){
				uni.request({
					url: 'https://www.ucharts.cn/data.json',
					data:{
					},
					success: function(res) {
						console.log(res.data.data)
						let Column={categories:[],series:[]};
						//这里我后台返回的是数组，所以用等于，如果您后台返回的是单条数据，需要push进去
						Column.categories=res.data.data.ColumnB.categories;
						Column.series=res.data.data.ColumnB.series;
						_self.textarea = JSON.stringify(res.data.data.ColumnB);
						_self.showColumn("canvasColumn",Column);
					},
					fail: () => {
						_self.tips="网络错误，小程序端请检查合法域名";
					},
				});
			},
			scroll: function(e) {
					this.scrollTop = e.detail.scrollTop
			},
			showColumn(canvasId,chartData){
				canvaColumn=new uCharts({
					$this:_self,
					canvasId: canvasId,
					type: 'column',
					padding:[15,5,0,15],
					legend:{
						show:true,
						padding:5,
						lineHeight:11,
						margin:0,
					},
					fontSize:11,
					background:'#FFFFFF',
					pixelRatio:_self.pixelRatio,
					animation: true,
					categories: chartData.categories,
					series: chartData.series,
					xAxis: {
						disableGrid:true,
					},
					yAxis: {
						data:[{
							position:'right',
							axisLine:false,
							format:(val)=>{return val.toFixed(0)+'元'},
						}]
					},
					dataLabel: true,
					width: _self.cWidth*_self.pixelRatio,
					height: _self.cHeight*_self.pixelRatio,
					extra: {
						column: {
							type:'group',
							width: _self.cWidth*_self.pixelRatio*0.45/chartData.categories.length
						}
						}
				});
			},
			changeData(){
				if(isJSON(_self.textarea)){
					let newdata=JSON.parse(_self.textarea);
					canvaColumn.updateData({
						series: newdata.series,
						categories: newdata.categories,
						animation:true
					});
				}else{
					uni.showToast({
						title:'数据格式错误',
						image:'../../../static/images/alert-warning.png'
					})
				}
			}
		}
	}
</script>

<style>
	/*样式的width和height一定要与定义的cWidth和cHeight相对应*/
	.qiun-charts {
		width: 750upx;
		height: 500upx;
		background-color: #FFFFFF;
	}
	
	.charts {
		width: 750upx;
		height: 500upx;
		background-color: #FFFFFF;
	}
</style>
