<template>
	<view class="custom-tabbar">
		<view 
			class="tab-item" 
			v-for="(item, index) in tabList" 
			:key="index"
			:class="{ 'tab-item-active': currentIndex === index }"
			@click="switchTab(index, item.pagePath)"
		>
			<view class="tab-icon">
				<text class="icon-text">{{ item.icon }}</text>
			</view>
			<text class="tab-text">{{ item.text }}</text>
		</view>
	</view>
</template>

<script>
export default {
	name: 'CustomTabbar',
	props: {
		current: {
			type: Number,
			default: 0
		}
	},
		data() {
		return {
			currentIndex: this.current,
			tabList: [
				{
					pagePath: '/pages/search/search',
					text: '搜索',
					icon: '🔍'
				},
				{
					pagePath: '/pages/pk/pk',
					text: 'PK',
					icon: '⚔️'
				},
				{
					pagePath: '/pages/ranking/ranking',
					text: 'CEO排行榜',
					icon: '🏆'
				},
				{
					pagePath: '/pages/parent/parent',
					text: '家长',
					icon: '👨‍👩‍👧'
				},
				{
					pagePath: '/pages/try/try',
					text: '试一试',
					icon: '✨'
				}
			]
		}
	},
	watch: {
		current(newVal) {
			this.currentIndex = newVal
		}
	},
	methods: {
		switchTab(index, pagePath) {
			if (this.currentIndex === index) return
			
			this.currentIndex = index
			this.$emit('change', index)
			
			uni.switchTab({
				url: pagePath,
				fail: () => {
					// 如果switchTab失败，使用navigateTo
					uni.navigateTo({
						url: pagePath
					})
				}
			})
		}
	}
}
</script>

<style scoped>
.custom-tabbar {
	position: fixed;
	bottom: 0;
	left: 0;
	right: 0;
	height: 110rpx;
	background: rgba(255, 255, 255, 0.95);
	backdrop-filter: blur(20rpx);
	border-top: 1rpx solid rgba(0, 206, 209, 0.3);
	display: flex;
	z-index: 1000;
	box-shadow: 0 -2rpx 20rpx rgba(0, 206, 209, 0.15);
}

.tab-item {
	flex: 1;
	display: flex;
	flex-direction: column;
	align-items: center;
	justify-content: center;
	padding: 8rpx 0;
	transition: all 0.2s ease;
	position: relative;
}

.tab-item-active {
	color: #00CED1;
}

.tab-icon {
	width: 48rpx;
	height: 48rpx;
	display: flex;
	align-items: center;
	justify-content: center;
	margin-bottom: 4rpx;
}

.icon-text {
	font-size: 32rpx;
	line-height: 1;
}

.tab-text {
	font-size: 20rpx;
	color: #535c68;
	line-height: 1;
	font-weight: 500;
}

.tab-item-active .tab-text {
	color: #00CED1;
	font-weight: 700;
}
</style>
