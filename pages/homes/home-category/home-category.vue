<template>
	<view>
		<view class="home_category">
			<navigator class="cate_item" 
			v-for="item in category" :key="item.id"
			:url="'/pages/homes/imgCategory/imgCategory?id={{item.id}}'">
				<image class="cate_img" :src="item.cover" mode="aspectFill"></image>
				<view class="cate_name">{{item.name}}</view>
			</navigator>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				category:[],
			}
		},
		methods: {
			getList(){
				this.request({
					url:'http://157.122.54.189:9088/image/v1/vertical/category'
				}).then(result => {
					console.log(result);
					this.category = result.res.category;
				})
			}
		},
		mounted() {
			uni.setNavigationBarTitle({
				title:"分类"
			});
			this.getList();
		}
	}
</script>

<style>
	.home_category{
		display: flex;
		flex-wrap: wrap;
	}
	.cate_item{
		width: 33.33%;
		position: relative;
		border: 5rpx solid #fff;
	}
	.cate_img{
		height: 240rpx;
	}
	.cate_name{
		position: absolute;
		left: 0;
		bottom: 0;
		color: #fff;
		background-image:linear-gradient(to right top,rgba(0,0,0,.2), rgba(0,0,0,0)) ;
		width: 100%;
		height: 50rpx;
		font-size: 40rpx;
		display: flex;
		align-items: center;
		padding-left: 20rpx;
	}

</style>
