<template>
	<view>
		<view class="home_tab">
			<view class="home_tab_title">
				<view class="title_inner">
					<uni-segmented-control :current="current" :values="items.map(v=>v.title)" @clickItem="onClickItem" style-type="text" active-color="#d4237a"></uni-segmented-control>			
				</view>
				<view class="icon">
					<icon type="search" size="20"/>
				</view>
			</view>
			<view class="home_tab_content">
				<view v-if="current === 0">
					<home-recommend></home-recommend>
				</view>
				<view v-if="current === 1">
					<home-category></home-category>
				</view>
				<view v-if="current === 2">
					<home-new></home-new>
				</view>
				<view v-if="current === 3">
					<home-album></home-album>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import homeAlbum from "../../pages/homes/home-album/home-album.vue";
	import homeCategory from "../../pages/homes/home-category/home-category.vue";
	import homeRecommend from "../../pages/homes/home-recommend/home-recommend.vue";
	import homeNew from "../../pages/homes/home-new/home-new.vue";
	import uniSegmentedControl from "@/components/uni-segmented-control/uni-segmented-control.vue";
	
	export default {
		components:{
			homeAlbum,
			homeCategory,
			homeRecommend,
			homeNew,
			uniSegmentedControl
		},
		data() {
			return {
				items: [
					{title:"推荐"},
					{title:"分类"},
					{title:"最新"},
					{title:"专辑"}
				],
				current: 0,
			}
		},
		methods: {
			onClickItem(e) {
				if (this.current !== e.currentIndex) {
					this.current = e.currentIndex;
				}
			}
		},
		onLoad() {
			
			
			/*
			//封装
			this.request({
				url:"http://157.122.54.189:9088/image/v3/homepage/vertical"
			}).then(res=>{
				console.log(res);
			})
			
			//1.原生的微信小程序的api
			wx.request({
				url:"http://157.122.54.189:9088/image/v3/homepage/vertical",
				success(res){
					console.log(res);
				}
			})
			//2.un-api
			uni.request({
				url:"http://157.122.54.189:9088/image/v3/homepage/vertical",
			}).then(res=>{console.log(res);})
			*/
		}
	}
</script>

<style>
	.home_tab{
	}
	
	.home_tab_title{
		position: relative;
	}
	.title_inner{
		width: 60%;
		margin: 0 auto;
	}
	.icon{
		position: absolute;
		top: 5%;
		right: 5%;
	}
</style>

