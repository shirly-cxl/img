<template>
	<view>
		<view class="album_bg">
			<image :src="album.cover" mode="widthFix"></image>
			<view class="album_info">
				<view class="album_name">{{album.name}}</view>
				<view class="album_btn">关注专辑</view>
			</view>
		</view>
		
		<view class="album_author">
			<view class="album_author_info">
				<image mode="widthFix" :src="album.user.avatar"></image>
				<view class="author_name">{{album.user.name}}</view>
			</view>
			<view class="album_author_desc">
				<text>{{album.desc}}</text>
			</view>
		</view>
		
		<view class="album_list">
			<view class="album_item" v-for="(item,index) in wallpaper" :key="item.id">
				<go-detail :list="wallpaper" :index="index">
					<image :src="item.thumb +item.rule.replace('$<Height>',360)" mode="widthFix"></image>
				</go-detail>
			</view>
		</view>
	</view>
</template>

<script>
	import {
		goDetail
	} from '../../../components/goDetail/goDetail'
	
	export default {
		components: {
			goDetail
		},
		data() {
			return {
				params: {
					limit: 30,
					order: "new",
					skip: 0,
					//1 返回值中有 album对象   表示第一次请求
					//0 返回值中只有 wallpaper数组   表示不是第一次请求
					first:1,
				},
				id:-1,
				album:{},
				wallpaper:[],
				hasMore:true,
			}
		},
		methods: {
			getlist() {
				this.request({
					url: "http://157.122.54.189:9088/image/v1/wallpaper/album/"+this.id+"/wallpaper",
					data: this.params,
				}).then(result => {
					
					if(Object.keys(this.album).length === 0){
						this.album = result.res.album;
					}
					if(result.res.wallpaper.length ===0){
						this.hasMore = false;
						uni.showToast({
							title: '没用数据了',
							icon: 'none'
						});
						return;
					}
					
					this.wallpaper = [...this.wallpaper,...result.res.wallpaper];
					console.log(result);
				})
			},
		},
		onLoad (options){
			console.log(options);
			this.id = options.id;
			// this.id ="5d5f8e45e7bce75ae7fb8278";
			this.getlist();
		},
		//页面触底 上拉加载下一页
		onReachBottom() {
			if(this.hasMore){
				this.params.first = 0;
				this.params.skip += this.params.limit;
				this.getlist();
			}else{
				uni.showToast({
					title: '没用数据了',
					icon: 'none'
				});
			}
		}
	}
</script>

<style>
	.album_bg{
		position: relative;
	}
	
	.album_info{
		position: absolute;
		width: 100%;
		left: 0;
		bottom: 0;
		display: flex;
		justify-content: space-between;
		align-items: center;
		height: 80rpx;
		color: #fff;
		padding: 0 15rpx;
	}
	
	.album_name{
		font-size: 40rpx;
	}
	
	.album_btn{
		background-color: #d52a7e;
		width: 150rpx;
		height: 60rpx;
		border-radius: 10rpx;
		display: flex;
		justify-content: center;
		align-items: center;
	}
	
	.album_author{
		padding: 10rpx 0;
		margin: 15rpx;
	}
	
	.album_author_info{
		padding: 10rpx 0;
		display: flex;
	}
	
	.album_author_info image{
		width: 50rpx;
	}
	
	.author_name{
		color: #000;
		margin-left: 15rpx;
	}
	
	.album_list{
		display: flex;
		flex-wrap: wrap;
	}
	
	.album_item{
		width: 33.33%;
		border: 3rpx solid #fff;
	}
</style>
