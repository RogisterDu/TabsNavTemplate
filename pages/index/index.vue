<template>
	<view class="content">
		<view class="uni-swiper-header">
			<view class="uni-tab-bar">
				<scroll-view scroll-x :scroll-into-view="'tab' + tabIndex" show-scrollbar="false">
					<view class="swiper-tab">
						<view :id="'tab' + indext" class="tabcontainer" v-for="(tab, indext) in tablist" :key="indext" @tap="tabtap(indext)">
							<text :class="[tabIndex == indext ? 'activeTab' : '', 'normalTab']">{{ tab.Name }}</text>
							<view :class="tabIndex == indext ? 'activeLine' : ''"></view>
						</view>
					</view>
				</scroll-view>
				<!-- 这里可以放置搜索按钮 begin 记得改动 -->
				<!-- 这里可以放置搜索按钮 end -->
			</view>
		</view>

		<swiper :current="tabIndex" :style="{ height: swiperheight + 'px' }" @change="swiperChange">
			<swiper-item class="wisdomActivity" v-for="(tabitem, tabs_i) in tablist" :key="tabs_i">
				<view>
					<scroll-view scroll-y="true" :style="{ height: swiperheight + 'px' }" @scrolltolower="getMore">
						<!-- Tabs内容 -->
						<text>这里是{{ tabitem.Name }}的内容</text>
						<view v-if="tabitem.noContent">该页无内容</view>
						<view v-else>内容如下</view>
					</scroll-view>
				</view>
			</swiper-item>
		</swiper>
	</view>
</template>

<script>
export default {
	data() {
		return {
			swiperheight: 0,
			tabIndex: 0,
			tablist: [
				{
					Name: '标签1',
					noContent: true, //Content是否有内容
					Content: []
				},
				{
					Name: '标签2',
					noContent: false,
					Content: []
				},
				{
					Name: '标签3',
					noContent: true,
					Content: []
				},
				{
					Name: '标签4',
					noContent: true,
					Content: []
				},
				{
					Name: '标签5',
					noContent: true,
					Content: []
				},
				{
					Name: '标签6',
					noContent: true,
					Content: []
				},
				{
					Name: '标签7',
					noContent: true,
					Content: []
				},
				{
					Name: '标签8',
					noContent: true,
					Content: []
				},
				{
					Name: '标签9',
					noContent: true,
					Content: []
				}
			]
		};
	},
	onLoad() {
		this.setSwiperHeight();
	},
	onReady() {
		// 初始化
		// this.getContentList(false);
	},
	methods: {
		tabtap(index) {
			this.tabIndex = index;
		},
		swiperChange(e) {
			this.tabIndex = e.detail.current;
		},
		setSwiperHeight() {
			let windowHeight = uni.getSystemInfoSync().windowHeight;
			var query = uni.createSelectorQuery();
			query
				.select('.uni-swiper-header')
				.boundingClientRect(rect => {
					if (rect) {
						this.swiperheight = windowHeight - rect.height;
					}
				})
				.exec();
		},
		getMore() {
			// 下拉加载更多
			// this.getContentList(true);
		},
		getContentList(isconcat) {
			//isconcat True下拉加载更多 false 刷新
			let offset = 0;
			let that = this;
			if (isconcat) {
				offset = that.tablist[that.tabIndex].Content;
			}
			// 假象接口  offset 10为获取内容长度
			GetContent(offset, 10).then(res => {
				let TmpList = that.tab;
				if (res.Success) {
					if (res.Data == null) {
						res.Data == {};
					}
					if (isconcat) {
						TmpList = TmpList.concat(res.Data);
					} else {
						TmpList = res.Data;
					}
					that.tablist[that.tabIndex].Content = TmpList;
				}
			});
		}
	}
};
</script>

<style lang="scss" scoped>
.content {
	background: #ffffff;
	min-height: 100vh;
	height: 100%;
}

.uni-swiper-header {
	background-color: #fff1f0;
	display: flex;
	flex-direction: row;
	align-items: center;
}

::-webkit-scrollbar {
	width: 0;
	height: 0;
	background-color: transparent;
}

.uni-swiper-tab {
	white-space: nowrap;
}

.uni-tab-bar {
	font-size: 28rpx;
	font-family: PingFangSC-Regular, PingFang SC;
	font-weight: 400;
	color: #333333;

	/* 导航右侧放置搜素按钮注释 替换*/
	width: 100%;
	// width: 640rpx;
}

.swiper-tab {
	padding: 0 32rpx 0 0;
	display: flex;
	align-items: center;
}

.activeTab {
	color: #e32415;
	font-weight: bold;
}

.normalTab {
	width: 128rpx;
	height: 48rpx;
	font-family: PingFangSC-Semibold;
	font-size: 32rpx;
	line-height: 48rpx;
}
.tabcontainer {
	padding: 24rpx 8rpx 20rpx 32rpx;
	position: relative;
	/* flex: 1; */
	text-align: center;
	display: flex;
	justify-content: center;
	align-content: center;
}

.activeLine {
	position: absolute;
	background-color: #e32415;
	width: 64rpx;
	height: 6rpx;
	bottom: 0rpx;
	/* margin:auto 0; */
}
</style>
