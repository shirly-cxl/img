<template>
	<view>
		<view class="video_tab">
			<view class="video_tab_title">
				<view class="title_inner">
					<uni-segmented-control :current="current" :values="items.map(v=>v.title)" @clickItem="onClickItem" style-type="text" active-color="#d4237a"></uni-segmented-control>			
				</view>
				<view class="icon">
					<icon type="search" size="20"/>
				</view>
			</view>
			<view class="video_tab_content">
				<view v-if="current < 4">
					<video-main :urlobj="{url:items[current].url,params:items[current].params}"></video-main>
				</view>
				<!-- <view v-if="current === 4">
					<video-category :urlobj="{url:items[current].url,params:items[current].params}"></video-category>
				</view> -->
			</view>
		</view>
	</view>
</template>

<script>
	import uniSegmentedControl from "@/components/uni-segmented-control/uni-segmented-control.vue";
	import videoCategory from "../../pages/videos/video_category/video_category.vue";
	import videoMain from "../../pages/videos/video_main/video_main.vue";
	
	export default {
		components:{
			uniSegmentedControl,
			videoCategory,
			videoMain
		},
		data() {
			return {
				items: [
					{
						title:"推荐",
						url:"http://157.122.54.189:9088/videoimg/v1/videowp/featured",
						params:{limit:30,skip:0,order:"hot"},
					},
					{
						title:"娱乐",
						url:"http://157.122.54.189:9088/videoimg/v1/videowp/category/59b25abbe7bce76bc834198a",
						params:{limit:30,skip:0,order:"new"},
					},
					{
						title:"最新",
						url:"http://157.122.54.189:9088/videoimg/v1/videowp/videowp",
						params:{limit:30,skip:0,order:"new"},
					},
					{
						title:"热门",
						url:"http://157.122.54.189:9088/videoimg/v1/videowp/videowp",
						params:{limit:30,skip:0,order:"hot"},
					},
					// {
					// 	title:"分类",
					// 	url:"http://157.122.54.189:9088/videoimg/v1/videowp/category",
					// 	params:{},
					// }
				],
				current: 0,
			}
		},
		methods: {
			onClickItem(e) {
				if (this.current !== e.currentIndex) {
					this.current = e.currentIndex;
				}
			},
			
		}
	}
</script>

<style>
	.video_tab_title{
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
