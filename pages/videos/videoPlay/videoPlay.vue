<template>
	<view class="video_play">
		<image class="video_img" :src="videoObj.img"></image>
		<view class="video_tool">
			<view @click="handleMusic" class="text">{{muted ? '静音' :'音量'}}</view>
			<view class="text share">
				转发
				<button class="btn" open-type="share"></button>
			</view>
		</view>
		<view class="video_wrap">
			<video class="video_width" :muted="muted" :src="videoObj.video" objectFit="fill"></video>
		</view>
		<view class="download">
			<view @click="handleDownload" class="download_btn">下载高清</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				videoObj:{},
				muted:false,
			}
		},
		onLoad() {
			// console.log(getApp().globalData.video);
			this.videoObj = getApp().globalData.video;
		},
		methods: {
			handleMusic(){
				this.muted =! this.muted;
			},
			
			async handleDownload(){
				await uni.showLoading({
					title:"下载中"
				});
				const {tempFilePath} = (await uni.downloadFile({url: this.videoObj.video}))[1];
				await uni.saveVideoToPhotosAlbum({filePath:tempFilePath});
				
				uni.hideLoading();
				await uni.showToast({
					title:'下载成功'
				});
			}
		}
	}
</script>

<style>
	.video_play{
		position: relative;
	}
	.video_img{
		position: absolute;
		width: 100vw;
		height: 100vh;
		filter: blur(20px);
		z-index: -1;
	}
	.video_tool{
		height: 80rpx;
		display: flex;
		justify-content: flex-end;
	}
	.text{
		width: 80rpx;
		color: #fff;
		font-size: 25rpx;
		border-radius: 40rpx;
		background-color: rgba(0,0,0,0.2);
		display: flex;
		justify-content: center;
		align-items: center;
		margin-right: 20rpx;
	}
	.share{
		position: relative;
	}
	.btn{
		position: absolute;
		width: 100%;
		height: 100%;
		opacity: 0;
	}
	.video_wrap{
		display: flex;
		justify-content: center;
	}
	.video_width{
		width: 360rpx;
		height: 600rpx;
	}
	.download{
		display: flex;
		justify-content: center;
		margin-top: 30rpx;
	}
	.download_btn{
		width: 360rpx;
		height: 80rpx;
		border-radius: 40rpx;
		display: flex;
		justify-content: center;
		align-items: center;
		color: #fff;
		border: 1rpx solid #fff;
		background-color:  rgba(0,0,0,0.2);
	}
</style>
