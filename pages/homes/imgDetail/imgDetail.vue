<template>
	<view>
		<view class="user_info">
			<view class="user_icon">
				<image :src="imgDetail.user.avatar" mode="widthFix"></image>
			</view>
			<view class="user_desc">
				<view class="user_name">{{imgDetail.user.name}}</view>
				<view class="user_time">{{imgDetail.atime}}</view>
			</view>
		</view>
		
		<view class="high_img">
			<image :src="imgDetail.newThumb" mode="widthFix"></image>
		</view>
		
		<view class="user_rank">
			<view class="rank">
				<text>点赞:{{imgDetail.rank}}</text>
			</view>
			<view class="user_collect">
				<text>收藏</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				imgDetail:{},
				
			}
		},
		methods: {
			
		},
		onLoad() {
			console.log(getApp().globalData);
			const {imgList,imgIndex} = getApp().globalData;
			this.imgDetail = imgList[imgIndex];
			this.imgDetail.newThumb = this.imgDetail.thumb + this.imgDetail.rule.replace('$<Height>',360)
		}
	}
</script>

<style>
	.user_info{
		display: flex;
		padding: 20rpx;
	}
	
	..user_icon{
		padding: 0 20rpx;
	}
	
	.user_icon image{
		width: 88rpx;
		border-radius: 50%;
	}
	
	.user_desc{
		margin-left: 20rpx;
	}
	
	.user_name{
		color: #000;
		font-weight: 600;
	}
	
	.user_time{
		color: #ccc;
		font-size: 24rpx;
		padding: 10rpx 0;
	}
	
	.user_rank{
		display: flex;
		height: 80rpx;
		border-bottom: 5rpx solid #eee;
	}
	
	.rank{
		flex:1;
		display: flex;
		justify-content: center;
		align-items: center;
	}
	
	.user_collect{
		flex:1;
		display: flex;
		justify-content: center;
		align-items: center;
	}
</style>
