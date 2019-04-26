<template>
	<view class="content">
		<view class="title-box">
            <text class="title">{{title}}</text>
        </view>
		<view class="uni-list">
			<view class="uni-list-cell" hover-class="uni-list-cell-hover">
				<view class="uni-list-cell-navigate uni-navigate-right">
					<navigator class="navigator" url="../lazy-load/lazy-load">懒加载</navigator>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: '懒加载实现'
			}
		},
		onLoad() {

		},
		methods: {

		}
	}
</script>

<style>
	.content {
		text-align: center;
		height: 400upx;
	}
	.title-box{
		height: 200upx;
		width: 100%;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: space-around;
	}
	.title {
		font-size: 36upx;
		color: #8f8f94;
	}
	.navigator{
		width: 100%;
		height: 100%;
	}
</style>
