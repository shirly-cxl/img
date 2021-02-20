<template>
	<view @click="handleClick">
		<slot></slot>
	</view>
</template>

<script>
	export default {
		props:{
			list:Array,
			index:Number,
		},
		data() {
			return {
				
			}
		},
		mounted(){
			console.log(this.list);
		},
		methods:{
			handleClick(){
				//将数据缓存下拉
				getApp().globalData.imgList = this.list;
				getApp().globalData.imgIndex = this.index;
				//点击跳转页面
				uni.navigateTo({
					url:"/pages/homes/imgDetail/imgDetail"
				})
			}
		}
	}
</script>

<style>

</style>
