<template>
	<view>
		<scroll-view scroll-y class="recommend_view" @scrolltolower="handleTolower">
			<view class="album_swiper">
				<swiper autoplay circular indicator-dots>
					<swiper-item v-for="item in banner" :key="item.id">
						<image :src="item.thumb"></image>
					</swiper-item>
				</swiper>
			</view>
			
			<view class="album_list">
				<navigator class="album_item" v-for="item in album" :key="item.id" :url="'/pages/homes/album/album?id={{item.id}}'">
					<view class="album_img">
						<image :src="item.cover" mode="aspectFill"></image>
					</view>
					<view class="album_info">
						<view class="album_name">{{item.name}}</view>
						<view class="album_desc">{{item.desc}}</view>
						<view class="album_btn">
							<view class="album_attention">关注</view>
						</view>
					</view>
				</navigator>
			</view>
		</scroll-view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				params: {
					//要获取几条
					limit: 30,
					//关键字
					order: "new",
					//要跳过几条
					skip: 0
				},
				banner: [],
				album: [],
				hasMore: true,
			}
		},
		methods: {
			getlist() {
				this.request({
					url: "http://157.122.54.189:9088/image/v1/wallpaper/album",
					data: this.params,
				}).then(result => {
					
					if(this.banner.length === 0){
						this.banner = result.res.banner;
					}
					if(result.res.album.length ===0){
						this.hasMore = false;
						uni.showToast({
							title: '没用数据了',
							icon: 'none'
						});
						return;
					}
					this.album = [...this.album,...result.res.album];
					console.log(result);
				})
			},
			
			handleTolower() {
				if (this.hasMore) {
					this.params.skip += this.params.limit;
					this.getlist();
				} else {
					uni.showToast({
						title: '没用数据了',
						icon: 'none'
					})
				}
			
			},
		},
		mounted() {
			//修改页面标题
			uni.setNavigationBarTitle({
				title: "专辑"
			});
			this.getlist();
		}
	}
</script>

<style>
	.recommend_view {
		/* height:屏幕高度 - 头部高度 */
		height: calc(100vh - 40px);
	}
	
	.album_swiper swiper{
		height: calc(750rpx / 2.3);
	}
	
	.album_swiper swiper image{
		height: 100%;
	}
	
	.album_list{
		padding: 10rpx;
	}
	
	.album_item{
		padding: 10rpx 0;
		display: flex;
		border-bottom: 1rpx solid #ccc;
	}
	
	.album_img{
		flex: 1;
		padding: 10rpx;
	}
	
	.album_img image{
		width: 200rpx;
		height: 200rpx;
	}
	
	.album_info{
		flex: 2;
		overflow: hidden;
		padding: 0 10rpx;
	}
	
	.album_name{
		font-size:34rpx;
		color:  #000;
		font-weight: bold;
		padding: 10rpx 0;
	}
	
	.album_desc{
		padding: 10rpx 0;
		font-size: 28rpx;
		text-overflow: ellipsis;
		overflow: hidden;
		white-space: nowrap;
	}
	
	.album_btn{
		padding: 10rpx;
		display: flex;
		justify-content: flex-end;
	}
	
	.album_attention{
		color: #d52a7e;
		font-size: 24rpx;
		border: 1rpx solid #d52a7e;
		padding: 10rpx ;
	}
</style>
