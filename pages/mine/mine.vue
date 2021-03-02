<template>
	<view class="page">
		<view class="head">
			<image src="../../static/bj.jpg" class="background" mode="widthFix"></image>
			<view class="userInfo">
				<view class="bg">
					<image class="act" v-if="login" :src="userInfo.avatarUrl"></image>
					<view class="img-bg" v-else>
						<image src="../../static/mine.png" class="img"></image>
					</view>
				</view>
				<view class="line">
					<view class="true" v-if="login">{{userInfo.nickName}}</view>
					<button class="login" v-else open-type="getUserInfo" lang="zh_CN" @getuserinfo="appLoginWx">点击登录</button >
				</view>
			</view>
		</view>
		
		<view v-for="item in mineList" :key="item.id">
			<view class="mine" @click="goDetailPage(item.url)">
				<!-- <image class="list" :src="item.img"></image> -->
				<view class="name">{{item.name}}</view>
				<image class="arrow" src="../../static/right.png"></image>
			</view>
		</view>
		
		<view  v-if="login" class="closeLogin">
			<view class="close" @tap="off">退出登錄</view>
		</view>
		
	</view>
</template>

<script>
	
	export default {
		
		data() {
			return {
				mineList:[
					{
						id:0,
						name:'我的收藏',
						url:'seal'
					},
					{
						id:1,
						name:'我的视频',
						url:'exchange'
					},
					{
						id:2,
						name:'设置',
						url:'prize'
					}
				],
				login:false,
				userInfo:[],
			}
		},
		methods: {
			
			goDetailPage(path) {
				uni.navigateTo({
					url: '/pages/mine/' + path + '/' + path
				});
			},
			
			off : function(){
				uni.showModal({
				   title: '退出登錄',
				   content: '是否要註銷賬號',
				   cancelText:'否',
				   confirmText:'是',
				   success:  (res) => {
				       if (res.confirm) {
						   uni.removeStorage({
								key: 'userInfo'  
						   });
						   uni.removeStorageSync('name');
						   uni.removeStorageSync('sessionkey');
						   this.login = false;
						   this.userInfo =[];
						   uni.showToast({
						       title: '退出成功',
						       duration: 2000
						   });
							 
				       } else if (res.cancel) {
						   this.login = true;
				           console.log('用户点击取消');
				       }
				   }
				});
				
			},
			
			appLoginWx(){
				
				uni.login({
					provider: 'weixin',
					success: (res) => {
					uni.getUserInfo({
							provider: 'weixin',
							success: (info) => {
								//异步存储
								uni.setStorage({
									key:"userInfo",
									data: info.userInfo 
								});
								uni.setStorageSync('name', info.userInfo.nickName);
								this.login = true;
								this.userInfo = info.userInfo;
							},
							fail: () => {}
						})
					},
				});
			}
		},
		onLoad() {
			this.appLoginWx();
		}
	}
</script>

<style>
	@import url("mine.css");
</style>
