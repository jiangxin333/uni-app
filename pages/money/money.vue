<template>
  <view>
    <view>
      <wuc-tab
        :tab-list="tabList"
        :tabCur.sync="TabCur"
        tab-class="text-center bg-white nav fixed"
        select-class="text-green"
        @change="tabChange"
		style="color: #666666;"
      ></wuc-tab>
		<view class="bg-white text-center text-black" style="margin: 2% 2% 10% 2%;">
			<mescroll-uni :down="downOption" @down="downCallback" :up="upOption" @up="upCallback" @init="mescrollInit">
				<view class="card" v-for="(item,idx) in infoList" :key="idx">
					<h3 class="title">{{item.title}}</h3>
					<view class="imgs">
						<!-- 根据图片数组长度显示图片 -->
						<template v-if="item.img_array.length >= 3">
							<image :style="smallStyle" class="img3" :src="item.img_array[1]" mode=""></image>
							<image :style="smallStyle" class="img3" :src="item.img_array[2]" mode=""></image>
							<image :style="smallStyle" class="img3" :src="item.img_array[0]" mode=""></image>
						</template>
						<template v-else>
							<image :style="bigStyle" class="img1" :src="item.img_array[0]" mode=""></image>
						</template>
					</view>
					<view class="footer">
						<view>
							<image style="width: 27upx; height: 27upx;" :src="item.icon" mode=""></image>
							<text style="color: red">{{item.tag_name}}</text>
						</view>
						<view style="color: red">{{item.view_money / 100}}/次</view>
						<view>{{item.pub_start_at}}</view>
					</view>
				</view>
			</mescroll-uni>
		</view>
    </view>
  </view>
</template>

<script>
import WucTab from "@/components/wuc-tab/wuc-tab.vue";
import MescrollUni from "mescroll-uni/mescroll-uni.vue";
export default {
	components: {
		WucTab,
		MescrollUni
	},
	data() {
		return {
			tabList: [{name: ''}],
			TabCur: 0,
			idx: 1,
			infoList: [],
			page: 1,
			//下拉刷新配置
			isShowNoMore: true,
			mescroll: null,
			downOption: { 
				use: true, // 是否启用下拉刷新; 默认true
				auto: false, // 是否在初始化完毕之后自动执行下拉刷新的回调; 默认true
				textOutOffset: '释放更新' // 下拉的距离大于offset范围的提示文本
			},
			upOption: {
				use: true, // 是否启用上拉加载; 默认true
				auto: false, // 是否在初始化完毕之后自动执行上拉加载的回调; 默认true
				textLoading: "加载中...",
				textNoMore: "没有更多数据了",
				toTop: {
					src : null , 
					offset : 700 , 
					duration : 800 
				},
			},
			smallStyle: {
				width:'32%',
				margin:'auto 0.66%',
				height:window.innerWidth * 0.3 * 135 / 229 + 'px',
				'border-radius':'6px',
				'background-color': '#000000'
			},
			bigStyle:{
				width:'100%',
				height:window.innerWidth*135/229+'px',
				'border-radius':'6px'
			}
		};
	},
	onReachBottom() {
		this.mescroll && this.mescroll.onReachBottom();
	},
	// 必须注册列表滚动事件,使下拉刷新生效	
	onPageScroll(e) {
		this.mescroll && this.mescroll.onPageScroll(e);
	},
	methods: {
		mescrollInit(mescroll) {
			this.mescroll = mescroll;
		},
		downCallback(mescroll) {
			// 这里加载你想下拉刷新的数据, 比如刷新轮播数据
			// loadSwiper();
			// 下拉刷新的回调,默认重置上拉加载列表为第一页 (自动执行 mescroll.num=1, 再触发upCallback方法 )
			this.mescroll.resetUpScroll();
			this.page = 1;
			this.upDate();
		},
		tabChange(index) {
			this.infoList = [];
			this.idx = this.tabList[index].id;
			this.page = 1;
			this.upDate();
		},
		upCallback() {
			this.page ++;
			this.upDate();
		},
		upDate() {
			uni.request({
				url: "http://migu.zmr016.com/article/list",
				method: 'GET',
				data: {
					type: this.idx,
					page: this.page
				},
				success: res => {
					if (res.data.data != '') {
							this.infoList = this.infoList.concat(res.data.data);
							this.mescroll.endSuccess();
							console.log(this.infoList,1111);
					} else {
						this.mescroll.endErr();
						// this.mescroll.endUpScroll(isShowNoMore)
					}
				},
			});
		}
	},
	//页面初始化生命周期函数
	onLoad() {
		uni.request({
			url: 'http://migu.zmr016.com/article/types',
			method: 'GET',
			success: res => {
				this.tabList = res.data.data;
			},
		});
		this.upDate();
	},
	mounted() {
		// console.log(this.tabList,777);
		// console.log(this.infoList,888);
	},
	onReady() {}
};
</script>
<style>
@import "../../components/demo/styles/main.scss";
.card {
	width: 100%;
}
.title {
	overflow:hidden;
    text-overflow:ellipsis;
    white-space:nowrap;
	font-weight: 200;
	font-family: "宋体";
	text-align: left;
}
.footer {
	display: flex;
	justify-content: space-between;
}
</style>