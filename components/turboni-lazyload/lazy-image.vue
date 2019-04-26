<template>
	<view class="lazy-image" :id="uuid">
		<image class="real-image" @load="onLoaded" :src="appear?realSrc:''" mode="aspectFill"></image>
		<view :class="loaded?'loaded':''">
			<image :src="placeholdSrc" mode="aspectFill"></image>
		</view>
	</view>
</template>

<script>
	export default {
		props:{
			placeholdSrc:{
				type:String,
				default:""
			},
			realSrc:{
				type:String,
				default:""
			}
		},
		data() {
			return {
				loaded:false,
				windowHeight:0,
				uuid:"",
				appear:false
			};
		},
		created() {
			this.uuid = this.makeUuid(32)
		},
		onLoad() {
			this.windowHeight = uni.getSystemInfoSync().windowHeight;
		},
		onReady(){
			this.checkAppear()
		},
		onPageScroll() {
			this.checkAppear()
		},
		methods:{
			checkAppear(){
				if(this.uuid){
					uni.createSelectorQuery().selectAll("#"+this.uuid).boundingClientRect((images) => {
						images.forEach((image, index) => {
							if (image.top <= this.windowHeight) {
								if(this.appear){
									return
								}
								this.appear = true;
								this.uuid = "";
							}
						})
					}).exec()
				}
			},
			onLoaded(){
				this.loaded = true;
			},
			makeUuid(len, radix) {
				var chars = '0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz'.split('');
				var uuid = [], i;
				radix = radix || chars.length;
				if (len) {
					for (i = 0; i < len; i++) uuid[i] = chars[0 | Math.random() * radix];
				} else {
					var r;
					uuid[8] = uuid[13] = uuid[18] = uuid[23] = '-';
					uuid[14] = '4';
					for (i = 0; i < 36; i++) {
						if (!uuid[i]) {
							r = 0 | Math.random() * 16;
							uuid[i] = chars[(i == 19) ? (r & 0x3) | 0x8 : r];
						}
					}
				}
				return uuid.join('');
			}
		}
	}
</script>

<style lang="scss" scoped>
	.lazy-image{
		height: 100%;
		width: 100%;
		flex: 1;
		display: flex;
		flex-direction: column;
		align-items: center;
		position: relative;
		image{
			width: 100%;
		}
		view{
			background-color: #eee;
			position: absolute;
			z-index: 2;
			top: 0;
			left: 0;
			height: 100%;
			width: 100%;
			flex: 1;
			display: flex;
			flex-direction: column;
			align-items: center;
			transition: opacity 0.4s linear;
			image{
				width: 100%;
			}
		}
		.loaded{
			opacity: 0;
		}
	}
</style>
