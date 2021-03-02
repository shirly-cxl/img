<template>
	<scroll-view scroll-y enable-flex @scrolltolower="handleScrolltolower" class="video_main">
		<view class="video_item" @click="handleVideo(item)"
		v-for="item in videowp" :key="item.id">
			<image :src="item.img" mode="widthFix"></image>
		</view>
	</scroll-view>
</template>

<script>
	export default {
		data() {
			return {
				videowp:[],
				hasMore:true,
			}
		},
		props:{
			urlobj:Object
		},
		watch:{
			urlobj(){
				console.log(this.urlobj);
				this.videowp = [];
				this.getList();
			}
		},
		mounted() {
			this.getList();
		},
		methods: {
			getList(){
				this.request({
					url: this.urlobj.url,
					data: this.urlobj.params
				}).then(result => {
					console.log(result);
					if(result.res.videowp.length === 0){
						this.hasMore = false;
						uni.showToast({
							title:"没有更多数据",
							icon:'none'
						});
						return;
					}
					this.videowp = [...this.videowp,...result.res.videowp];
					console.log(result);
				})
			},
			
			handleScrolltolower(){
				if(this.hasMore){
					this.urlobj.params.skip += this.urlobj.params.limit;
					this.getList();
				}else{
					uni.showToast({
						title:"没有更多数据",
						icon:'none'
					})
				}
			},
			
			handleVideo(item){
				getApp().globalData.video = item;
				uni.navigateTo({
					url:"/pages/videos/videoPlay/videoPlay"
				})
			}
		}
	}
</script>

<style>
	.video_main{
		display: flex;
		flex-wrap: wrap;
		height: calc(100vh - 36px);
	}
	.video_item{
		width: 33.33%;
		border: 5rpx solid #fff;
	}
</style>
