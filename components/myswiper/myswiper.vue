<template>
	<view  @touchstart ="handleTouchstart" @touchend="handleTouchend">
		<slot></slot>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				//按下事件
				startTime:0,
				//按下坐标
				startX:0,
				startY:0,
			}
		},
		methods: {
			handleTouchstart(event){
				// console.log("按下"+event.changedTouches[0].clientX);
				// console.log("按下"+event.changedTouches[0].clientY);
				this.startTime = Date.now();
				this.startX = event.changedTouches[0].clientX;
				this.startY = event.changedTouches[0].clientY;
			},
			
			handleTouchend(event){
				// console.log("离开"+event.changedTouches[0].clientX);
				// console.log("离开"+event.changedTouches[0].clientX);
				const endTime = Date.now();
				const endX = event.changedTouches[0].clientX;
				const endY = event.changedTouches[0].clientY;
				let direction="";
				//判断按下时长
				if(endTime - this.startTime >2000){
					return;
				}
				//先判断用户滑动的距离是否合法，在判断滑动方向
				if(Math.abs(endX - this.startX) > 10 && Math.abs(endY - this.startY) < 10){
					direction = endX - this.startX > 0 ? "right" : "left";
				}else{
					return;
				}
				//用户做了滑动操作
				// console.log(direction);
				this.$emit("myswiper",{direction});
			},
		}
	}
</script>

<style>
	
</style>
