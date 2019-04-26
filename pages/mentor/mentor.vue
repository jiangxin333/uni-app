<template>
	<view class="mentor">
		<view class="bnImg">
			<image src="../../static/img/news.jpg" mode=""></image>
		</view>
		<view style="text-align: center;">
			<text class="menterTilter">点击立即收徒</text>
		</view>
	</view>
</template>

<script>
	import uniNavBar from "@/components/uni-nav-bar/uni-nav-bar.vue"
	export default {
		components: {
			uniNavBar
		},
		data() {
			return {
			}
		},
		methods: {
			update() {
				console.log(1111)
			},
			sedate() {
				console.log(222);
			}
		},
		created() {
			this.update();
			this.sedate();
		}
		
	}
</script>

<style scoped>
	.bnImg image {
		width: 100%;
	}
	.menterTilter {
		background-color: #f44;
		display: inline-block;
		height: 60upx;
		width: 280upx;
		border-radius: 60upx;
		color: #fff;
		line-height: 60upx;
		font-size: 30upx;
	}
</style>
