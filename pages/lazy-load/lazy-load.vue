<template>
	<view>
		<view class="img-list" v-for="(item,index) in list" :key="index">
			<lazy-image class="lazy-image" :realSrc="item.src" :placeholdSrc="placeholderSrc"></lazy-image>
		</view>
	</view>
</template>

<script>
	import lazyImage from "@/components/lazy-image.vue"
	export default {
		components:{
			lazyImage
		},
		data() {
			var imgs = ['shuijiao', 'muwu', 'cbd']
			var list = []
			
			for (let i = 0; i < 20; i++) {
				list.push({
					src: `https://img-cdn-qiniu.dcloud.net.cn/uniapp/images/${imgs[i%3]}.jpg`,
					show: false,
					loaded: false
				})
			}
			return {
				placeholderSrc: '../../../static/60x60.png',
				list: list,
			}
		}
	}
</script>

<style>
.img-list{
	width: 750upx;
	height:300upx;
	margin-bottom: 20upx;;
}
</style>
