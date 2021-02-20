<template>
	<view v-if="recommends.length>0">
		<!-- ctrl+h 替换 -->
		<scroll-view scroll-y class="recommend_view" @scrolltolower="handleTolower">
			<!--推荐-->
			<view class="recommend_wrap">
				<navigator class="recommend_item" v-for="item in recommends" :key="item.id" :url="'/pages/homes/album/album?id={{item.target}}'">
					<image :src="item.thumb" mode="widthFix"></image>
				</navigator>
			</view>
			<!--月份-->
			<view class="months_wrap">
				<view class="months_title">
					<view class="months_title_info">
						<view class="months_info">
							<text> {{months.DD}} /</text>
							{{months.MM}}月
							<!-- 01 月 -->
						</view>
						<!-- <view class="months_text">你负责美丽就好</view> -->
						<view class="months_text">{{months.title}}</view>
					</view>
					<view class="months_more">更多 ></view>
				</view>
				<view class="months_content">
					<!-- <view class="months_item" v-for="item in monthsImg" :key="item.id">
					<image :src="item.img" mode="aspectFill"></image>
				</view> -->
					<view class="months_item" v-for="(items,index) in months.items" :key="items.id">
						<go-detail :list="months.items" :index="index">
							<image :src="items.thumb + items.rule.replace('$<Height>',360)" mode="aspectFill"></image>
						</go-detail>
					</view>
				</view>
			</view>
			<!--热门-->
			<view class="hots_wrap">
				<view class="hots_title">
					<text> 热门 </text>
				</view>
				<view class="hots_content">
					<view class="hots_item" v-for="(item,index) in hots" :key="item.id">
						<go-detail :list="hots" :index="index">
							<image :src="item.thumb" mode="aspectFill"></image>
						</go-detail>
					</view>
				</view>
			</view>
		</scroll-view>
	</view>
</template>

<script>
	import moment from '../../../utils/moment.js';
	import {
		goDetail
	} from '../../../components/goDetail/goDetail'
	export default {
		components: {
			goDetail
		},
		data() {
			return {
				// recommends: [{
				// 		id: 0,
				// 		thumb: 'http://img0.adesk.com/download/5a6b127de7bce724f43e8b22',
				// 	},
				// 	{
				// 		id: 1,
				// 		thumb: 'http://img5.adesk.com/5e05dd23e7bce739db12622e',
				// 	},
				// 	{
				// 		id: 2,
				// 		thumb: 'http://img0.adesk.com/download/59c319dfe7bce729eca94d41',
				// 	},
				// 	{
				// 		id: 3,
				// 		thumb: 'http://img5.adesk.com/5e05dd1ee7bce739c4b3bd20',
				// 	}
				// ],
				// monthsImg: [{
				// 		id: 0,
				// 		img: 'http://img0.adesk.com/download/5a6b127de7bce724f43e8b22',
				// 	},
				// 	{
				// 		id: 1,
				// 		img: 'http://img5.adesk.com/5e05dd23e7bce739db12622e',
				// 	},
				// 	{
				// 		id: 2,
				// 		img: 'http://img0.adesk.com/download/59c319dfe7bce729eca94d41',
				// 	},
				// 	{
				// 		id: 3,
				// 		img: 'http://img5.adesk.com/5e05dd1ee7bce739c4b3bd20',
				// 	},
				// 	{
				// 		id: 4,
				// 		img: 'http://img5.adesk.com/5e05dd23e7bce739db12622e',
				// 	},
				// 	{
				// 		id: 5,
				// 		img: 'http://img0.adesk.com/download/59c319dfe7bce729eca94d41',
				// 	}
				// ],
				// hots: [{
				// 		id: 0,
				// 		src: 'https://ss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=2133611660,2911693279&fm=26&gp=0.jpg',
				// 	},
				// 	{
				// 		id: 1,
				// 		src: 'https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=289770192,3166095963&fm=26&gp=0.jpg',
				// 	},
				// 	{
				// 		id: 2,
				// 		src: 'https://ss1.bdstatic.com/70cFvXSh_Q1YnxGkpoWK1HF6hhy/it/u=1191879488,2228639105&fm=26&gp=0.jpg',
				// 	},
				// 	{
				// 		id: 3,
				// 		src: 'https://ss2.bdstatic.com/70cFvnSh_Q1YnxGkpoWK1HF6hhy/it/u=2502594423,1136464949&fm=26&gp=0.jpg',
				// 	},
				// 	{
				// 		id: 4,
				// 		src: 'https://ss2.bdstatic.com/70cFvnSh_Q1YnxGkpoWK1HF6hhy/it/u=2827710282,660374534&fm=26&gp=0.jpg',
				// 	},
				// 	{
				// 		id: 5,
				// 		src: 'https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=1634195289,667542114&fm=26&gp=0.jpg',
				// 	}
				// ],
				// hotsNew: [{
				// 		id: 6,
				// 		src: 'https://ss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=2133611660,2911693279&fm=26&gp=0.jpg',
				// 	},
				// 	{
				// 		id: 7,
				// 		src: 'https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=289770192,3166095963&fm=26&gp=0.jpg',
				// 	},
				// 	{
				// 		id: 8,
				// 		src: 'https://ss1.bdstatic.com/70cFvXSh_Q1YnxGkpoWK1HF6hhy/it/u=1191879488,2228639105&fm=26&gp=0.jpg',
				// 	},
				// 	{
				// 		id: 9,
				// 		src: 'https://ss2.bdstatic.com/70cFvnSh_Q1YnxGkpoWK1HF6hhy/it/u=2502594423,1136464949&fm=26&gp=0.jpg',
				// 	},
				// 	{
				// 		id: 10,
				// 		src: 'https://ss2.bdstatic.com/70cFvnSh_Q1YnxGkpoWK1HF6hhy/it/u=2827710282,660374534&fm=26&gp=0.jpg',
				// 	},
				// 	{
				// 		id: 11,
				// 		src: 'https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=1634195289,667542114&fm=26&gp=0.jpg',
				// 	}
				// ]

				// 推荐列表
				recommends: [],
				// 月份
				months: {}, //对象
				// 热门列表
				hots: [],
				// 请求的参数
				params: {
					//要获取几条
					limit: 30,
					//关键字
					order: "hot",
					//要跳过几条
					skip: 0
				},
				// 是否还有下一页
				hasMore: true,
			}
		},
		methods: {
			//滚动条触底事件
			handleTolower() {
				console.log('滚动条触底!!');
				// this.hots = [...this.hots, ...this.hotsNew];
				/*
				1.修改参数   skip+=limit
				2.重新发送请求  getlist()
				3.请求回来成功  hots 数据的叠加
				*/
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

			//获取接口的数据

			getlist() {
				this.request({
					url: "http://157.122.54.189:9088/image/v3/homepage/vertical",
					data: this.params,
				}).then(result => {

					// 判断还有没有下一页数据
					if (result.res.vertical.length === 0) {
						this.hasMore = false;
						uni.showToast({
							title: '没用数据了',
							icon: 'none'
						});
						return;
					}
					console.log(result);
					if (this.recommends.length === 0) {
						// 第一次发送的请求
						// 推荐模块
						this.recommends = result.res.homepage[1].items;
						// 月份模块
						this.months = result.res.homepage[2];
						// 讲时间戳改成 18号/月  官网:http://momentjs.cn/
						this.months.MM = moment(this.months.stime).format("MM");
						this.months.DD = moment(this.months.stime).format("DD");
					}

					// 热门列表
					// this.hots = result.res.vertical;
					// 数组拼接 es6
					this.hots = [...this.hots, ...result.res.vertical];
				});
			}

		},
		// 触发
		mounted() {
			this.getlist();
			/*
			this.getlist();
			 
		    // 或者
		
			this.request({
				url:"http://157.122.54.189:9088/image/v3/homepage/vertical",		
				data:{
					//要获取几条
					limit:30,
					//关键字
					order:"hot",
					//要跳过几条
					skip:0
				}	
			}).then(result=>{
				console.log(result);
				// 推荐模块
				this.recommends=result.res.homepage[1].items;
				// 月份模块
				this.months=result.res.homepage[2];
				// 讲时间戳改成 18号/月  官网:http://momentjs.cn/
				this.months.MM=moment(this.months.stime).format("MM");
				this.months.DD=moment(this.months.stime).format("DD");
				// 热门列表
				this.hots=result.res.vertical;
				});
				*/
		}
	}
</script>

<style scoped>
	.recommend_view {
		/* height:屏幕高度 - 头部高度 */
		height: calc(100vh - 40px);
	}

	.recommend_wrap {
		display: flex;
		flex-wrap: wrap;
	}

	.recommend_item {
		width: 50%;
		border: 10rpx solid #fff;
	}

	.months_title {
		display: flex;
		justify-content: space-between;
		padding: 20rpx;
	}

	.months_title_info {
		color: #d52a7e;
		font-size: 30rpx;
		font-weight: 600;
		display: flex;
	}

	.months_info text {
		font-size: 38rpx;
	}

	.months_text {
		font-size: 34rpx;
		color: #666;
		margin-left: 30rpx;
	}

	.months_more {
		font-size: 26rpx;
		color: #d52a7e;
	}

	.months_content {
		display: flex;
		flex-wrap: wrap;
	}

	.months_item {
		width: 33.33%;
		border: 10rpx solid #fff;
	}

	.hots_wrap {}

	.hots_title {
		padding: 20rpx;
	}

	.hots_title text {
		border-left: 10rpx solid #d52a7e;
		padding-left: 20rpx;
		font-size: 36rpx;
		font-weight: 600;
	}

	.hots_content {
		display: flex;
		flex-wrap: wrap;
	}

	.hots_item {
		width: 33.33%;
		border: 5rpx solid #fff;
	}
</style>
