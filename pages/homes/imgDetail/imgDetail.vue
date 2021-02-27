<template>
	<view>
		<view class="user_info">
			<view class="user_icon">
				<image :src="imgDetail.user.avatar" mode="widthFix"></image>
			</view>
			<view class="user_desc">
				<view class="user_name">{{imgDetail.user.name}}</view>
				<view class="user_time">{{imgDetail.crTime}}</view>
			</view>
		</view>
		
		<view class="high_img">
			<myswiper @myswiper="handleMySwiper">
				<image :src="imgDetail.thumb" mode="widthFix"></image>
			</myswiper>
		</view>
		
		<view class="user_rank">
			<view class="rank">
				<text>点赞:{{imgDetail.rank}}</text>
			</view>
			<view class="user_collect">
				<text>收藏</text>
			</view>
		</view>
		
		<view class="album_wrap" v-if="album.length">
			<view class="album_title">相关</view>
			<view class="album_list">
				<view class="album_item" v-for="item in album" :key="item.id">
					<view class="album_cover">
						<image :src="item.cover" mode="aspectFill"></image>
					</view>
					<view class="album_info">
						<view class="album_text">专辑</view>
						<view class="album_name">{{item.name}}</view>
						<image class="img" src="https://ss0.bdstatic.com/70cFvHSh_Q1YnxGkpoWK1HF6hhy/it/u=2721696843,1547846631&fm=26&gp=0.jpg" style="width: 50rpx;height: 50rpx;" ></image>
					</view>
				</view>
			</view>
		</view>
		
		<view class="comment" v-if="hot.length">
			<view class="comment_title">
				<text class="comment_text">最热评论</text>
			</view>
			<view class="comment_list">
				<view class="comment_item" v-for="item in hot" :key="item.id">
					<view class="comment_user">
						<image class="image1" :src="item.user.avatar" mode="widthFix"></image>
						<view class="user2_name">
							<view class="user2_nickname">{{item.user.name}}</view>
							<view class="user2_time">{{item.cnTime}}</view>
						</view>
						<view class="usre2_badge">
							<image class="image2" v-for="item2 in item.user.title" :key="item2.id" :src="item2.icon"></image>
						</view>
					</view>
					<view class="comment_desc">
						<view class="comment_content">{{item.content}}</view>
						<view class="comment_like">点赞：{{item.size}}</view>
					</view>
				</view>
			</view>
		</view>
		
		<view class="comment" v-if="comment.length">
			<view class="comment_title">
				<text class="comment_text">最新评论</text>
			</view>
			<view class="comment_list">
				<view class="comment_item" v-for="item in comment" :key="item.id">
					<view class="comment_user">
						<image class="image1" :src="item.user.avatar" mode="widthFix"></image>
						<view class="user2_name">
							<view class="user2_nickname">{{item.user.name}}</view>
							<view class="user2_time">{{item.cnTime}}</view>
						</view>
						<view class="usre2_badge">
							<image class="image2" v-for="item2 in item.user.title" :key="item2.id" :src="item2.icon"></image>
						</view>
					</view>
					<view class="comment_desc">
						<view class="comment_content">{{item.content}}</view>
						<view class="comment_like">点赞：{{item.size}}</view>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import moment from "../../../utils/moment.js";
	import myswiper from "../../../components/myswiper/myswiper.vue";
	//设置语言为中文
	// moment.locale('zh-cn');
	export default {
		components:{
			myswiper
		},
		data() {
			return {
				imgDetail:{},
				//专辑数据
				album:[],
				//最热评论
				hot:[],
				//最新评论
				comment:[],
				//图片索引
				imgIndex:0,
			}
		},
		onLoad() {
			// console.log(getApp().globalData);
			const {imgIndex} = getApp().globalData;
			this.imgIndex = imgIndex;
			this.getDate();
		
		},
		methods: {
			getDate(){
				const {imgList} = getApp().globalData;
				this.imgDetail = imgList[this.imgIndex];
				// this.imgDetail.newThumb = this.imgDetail.thumb + this.imgDetail.rule.replace('$<Height>',360)
				//xxx年前的数据
				this.imgDetail.crTime =moment(this.imgDetail.atime*1000).fromNow();
				//获取图片详情id
				// this.imgDetail.id
				this.getComments(this.imgDetail.id);
			},
			
			getComments(id){
				this.request({
					url:'http://157.122.54.189:9088/image/v2/wallpaper/wallpaper/'+id+'/comment'
				}).then(result => {
					console.log(result);
					this.album = result.res.album;
					//给hot添加时间属性 xx月前
					result.res.hot.forEach(v => v.cnTime = moment(v.atime*1000).fromNow());
					this.hot = result.res.hot;
					result.res.comment.forEach(v => v.cnTime = moment(v.atime*1000).fromNow());
					this.comment = result.res.comment;
				})
			},
			
			handleMySwiper(e){
				// console.log(e);
				const {imgList} = getApp().globalData;
				//左滑 imgIndex++
				if(e.direction === "left" && this.imgIndex < imgList.length -1){
					this.imgIndex ++;
					this.getDate();
				}else if(e.direction === "right" && this.imgIndex >0){
					this.imgIndex --;
					this.getDate();
				}else{
					uni.showToast({
						title:'没有数据了',
						icon:'none'
					})
				}
			}
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
	/* 专辑 */
	.album_wrap{
		padding: 20rpx;
	}
	.album_title{
		padding: 10rpx 0;
	}
	
	.album_item{
		display: flex;
		padding: 10rpx 0;
		border-bottom: 4rpx solid #eee;
	}
	.album_cover{
		flex:1;
	}
	.album_cover image{
		width: 180rpx;
		height: 180rpx;
	}
	
	.album_info{
		flex:3;
		padding-left: 20rpx;
		position: relative;
	}
	.album_text{
		width: 100rpx;
		height: 50rpx;
		background-color:#d52a7e;
		color: #fff;
		display: flex;
		justify-content: center;
		align-items: center;
	}
	.album_name{
		padding: 10rpx 0;
		color: #888;
	}
	.img{
		position: absolute;
		top: 50%;
		right: 10%;
		transform: translateY(-50%);
	}
	
	/* 评论 */
	.comment{}
	.comment_title{
		padding: 15rpx;
	}
	.comment_text{
		font-weight: 600;
		font-size: 28rpx;
		/* margin-left: 10rpx; */
	}
	
	.comment_item{
		border-bottom: 15rpx solid #eee;
		padding: 16rpx;
	}
	.comment_user{
		display: flex;
		flex-direction: row;
		justify-content: center;
		align-items: center;
		padding: 20rpx 0;
	}
	 .image1{
		width: 15%;
	} 
	.user2_name{
		flex:1;
		padding-left: 10rpx;
	}
	.user2_nickname{
		color: #777;
	}
	.user2_time{
		color: #ccc;
		font-size: 24rpx;
		padding: 5rpx;
	}
	.image2{
		width: 40rpx;
		height: 40rpx;
	}
	.comment_desc{
		display: flex;
		padding: 30rpx 0;
	}
	.comment_content{
		flex: 1;
		padding-left: 15%;
		color: #000000;
	}
	.comment_like{
		text-align: right;
	}
</style>
