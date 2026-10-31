<template>
	<view class="gallery-container">
		<!-- 自定义导航栏 -->
		<view class="custom-navbar">
			<view class="navbar-content">
				<text class="navbar-title">AI作品展馆</text>
			</view>
		</view>
		
		<!-- 主体内容 -->
		<scroll-view class="content" scroll-y>
			<!-- 搜索栏 -->
			<view class="search-section">
				<view class="search-box">
					<input 
						class="search-input" 
						placeholder="搜索作品..." 
						v-model="searchKeyword"
						@input="onSearch"
					/>
					<text class="search-icon">🔍</text>
				</view>
			</view>
			
			<!-- 作品网格 -->
			<view class="gallery-grid">
				<view 
					class="gallery-item" 
					v-for="(item, index) in filteredGallery" 
					:key="index"
					@click="viewWork(item)"
				>
					<image class="gallery-cover" :src="item.cover" mode="aspectFill"></image>
					<view class="gallery-info">
						<text class="gallery-title">{{ item.title }}</text>
						<view class="gallery-meta">
							<text class="gallery-likes">❤️ {{ item.likes }}</text>
							<text class="gallery-date">{{ item.date }}</text>
						</view>
					</view>
				</view>
			</view>
			
			<!-- 上传按钮 -->
			<view class="upload-section">
				<button class="upload-btn" @click="goToUpload">
					<text class="upload-text">+ 上传新作品</text>
				</button>
			</view>
		</scroll-view>
		
		<!-- 自定义底部导航栏 -->
		<custom-tabbar :current="2" @change="onTabChange"></custom-tabbar>
		
		<!-- 作品详情弹窗 -->
		<view class="work-modal" v-if="showWorkDetail" @click="closeWorkDetail">
			<view class="work-content" @click.stop>
				<view class="work-header">
					<text class="work-title">{{ selectedWork.title }}</text>
					<text class="work-close" @click="closeWorkDetail">×</text>
				</view>
				<view class="work-body">
					<image class="work-image" :src="selectedWork.cover" mode="aspectFit"></image>
					<view class="work-description">
						<text class="work-desc-text">{{ selectedWork.description }}</text>
					</view>
					<view class="work-ai-quote">
						<text class="ai-quote-label">AI语录：</text>
						<text class="ai-quote-text">{{ selectedWork.aiQuote }}</text>
					</view>
					<view class="work-actions">
						<button class="action-btn like" @click="likeWork">
							<text class="btn-text">❤️ {{ selectedWork.likes }}</text>
						</button>
						<button class="action-btn share" @click="shareWork">
							<text class="btn-text">分享</text>
						</button>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
import CustomTabbar from '@/components/custom-tabbar.vue'

export default {
	components: {
		CustomTabbar
	},
	data() {
		return {
			searchKeyword: '',
			showWorkDetail: false,
			selectedWork: {},
			galleryData: [
				{
					id: 1,
					title: 'AI绘画作品 - 未来城市',
					cover: '/static/work1.jpg',
					likes: 128,
					date: '2天前',
					description: '使用AI技术创作的未来城市概念图，展现了科技与自然的和谐共生。',
					aiQuote: '每一笔都是算法与创意的完美融合，这就是AI艺术的魅力所在。'
				},
				{
					id: 2,
					title: '机器学习模型可视化',
					cover: '/static/work2.jpg',
					likes: 95,
					date: '5天前',
					description: '将复杂的机器学习模型以直观的方式展现出来，让技术更加易懂。',
					aiQuote: '数据是新的石油，而可视化是开采它的最佳工具。'
				},
				{
					id: 3,
					title: '自然语言处理项目',
					cover: '/static/work3.jpg',
					likes: 76,
					date: '1周前',
					description: '基于Transformer架构的文本生成模型，能够创作诗歌和散文。',
					aiQuote: '语言是思想的载体，AI让语言的艺术更加丰富多彩。'
				},
				{
					id: 4,
					title: '计算机视觉应用',
					cover: '/static/work4.jpg',
					likes: 142,
					date: '2周前',
					description: '智能图像识别系统，能够准确识别各种物体和场景。',
					aiQuote: '让机器拥有眼睛，让AI看见世界的美好。'
				},
				{
					id: 5,
					title: '深度学习实验',
					cover: '/static/work5.jpg',
					likes: 88,
					date: '3周前',
					description: '探索深度神经网络的各种架构和优化方法。',
					aiQuote: '深度学习就像探索未知的宇宙，每一层都是新的发现。'
				},
				{
					id: 6,
					title: 'AI音乐创作',
					cover: '/static/work6.jpg',
					likes: 156,
					date: '1个月前',
					description: '使用AI算法创作的音乐作品，融合了古典与现代元素。',
					aiQuote: '音乐是情感的语言，AI让这种语言更加丰富动人。'
				}
			]
		}
	},
	computed: {
		filteredGallery() {
			if (!this.searchKeyword) {
				return this.galleryData
			}
			return this.galleryData.filter(item => 
				item.title.toLowerCase().includes(this.searchKeyword.toLowerCase()) ||
				item.description.toLowerCase().includes(this.searchKeyword.toLowerCase())
			)
		}
	},
	onLoad() {
		this.loadGalleryData()
	},
	methods: {
		loadGalleryData() {
			// 模拟加载作品数据
			// 这里可以从API获取真实数据
		},
		onSearch() {
			// 搜索功能已在computed中实现
		},
		viewWork(work) {
			this.selectedWork = work
			this.showWorkDetail = true
		},
		closeWorkDetail() {
			this.showWorkDetail = false
		},
		likeWork() {
			this.selectedWork.likes++
			uni.showToast({
				title: '已点赞！',
				icon: 'success'
			})
		},
		shareWork() {
			uni.showToast({
				title: '分享功能开发中...',
				icon: 'none'
			})
		},
		goToUpload() {
			uni.navigateTo({
				url: '/pages/upload/upload'
			})
		},
		onTabChange(index) {
			// 处理底部导航栏切换
			console.log('Tab changed to:', index)
		}
	}
}
</script>

<style scoped>
.gallery-container {
	min-height: 100vh;
	background: linear-gradient(135deg, #f8f9fa 0%, #e9ecef 100%);
}

.custom-navbar {
	position: fixed;
	top: 0;
	left: 0;
	right: 0;
	z-index: 1000;
	background: rgba(255, 255, 255, 0.95);
	backdrop-filter: blur(10px);
	border-bottom: 1px solid rgba(0, 0, 0, 0.05);
}

.navbar-content {
	height: 88rpx;
	display: flex;
	align-items: center;
	justify-content: center;
	padding: 0 32rpx;
	padding-top: var(--status-bar-height);
}

.navbar-title {
	font-size: 36rpx;
	font-weight: 600;
	color: #2c3e50;
}

.content {
	margin-top: 88rpx;
	padding: 32rpx;
	padding-bottom: 120rpx;
}

.search-section {
	margin-bottom: 32rpx;
}

.search-box {
	position: relative;
	background: #ffffff;
	border-radius: 24rpx;
	box-shadow: 0 8rpx 32rpx rgba(0, 0, 0, 0.08);
	border: 1px solid rgba(0, 0, 0, 0.05);
}

.search-input {
	width: 100%;
	height: 88rpx;
	padding: 0 32rpx 0 64rpx;
	font-size: 28rpx;
	color: #2c3e50;
	border: none;
	background: transparent;
}

.search-icon {
	position: absolute;
	left: 32rpx;
	top: 50%;
	transform: translateY(-50%);
	font-size: 32rpx;
	color: #7f8c8d;
}

.gallery-grid {
	display: grid;
	grid-template-columns: 1fr 1fr;
	gap: 24rpx;
	margin-bottom: 40rpx;
}

.gallery-item {
	background: #ffffff;
	border-radius: 20rpx;
	overflow: hidden;
	box-shadow: 0 8rpx 32rpx rgba(0, 0, 0, 0.08);
	border: 1px solid rgba(0, 0, 0, 0.05);
	transition: transform 0.2s ease;
}

.gallery-item:active {
	transform: scale(0.98);
}

.gallery-cover {
	width: 100%;
	height: 200rpx;
	background: #f8f9fa;
}

.gallery-info {
	padding: 20rpx;
}

.gallery-title {
	display: block;
	font-size: 24rpx;
	font-weight: 600;
	color: #2c3e50;
	margin-bottom: 12rpx;
	line-height: 1.4;
	overflow: hidden;
	text-overflow: ellipsis;
	display: -webkit-box;
	-webkit-line-clamp: 2;
	-webkit-box-orient: vertical;
}

.gallery-meta {
	display: flex;
	justify-content: space-between;
	align-items: center;
}

.gallery-likes {
	font-size: 20rpx;
	color: #e74c3c;
}

.gallery-date {
	font-size: 20rpx;
	color: #7f8c8d;
}

.upload-section {
	display: flex;
	justify-content: center;
}

.upload-btn {
	width: 100%;
	height: 88rpx;
	background: linear-gradient(135deg, #3498db, #2980b9);
	color: #ffffff;
	border: none;
	border-radius: 44rpx;
	font-size: 32rpx;
	font-weight: 600;
	display: flex;
	align-items: center;
	justify-content: center;
	box-shadow: 0 8rpx 24rpx rgba(52, 152, 219, 0.3);
	transition: all 0.2s ease;
}

.upload-btn:active {
	transform: scale(0.98);
}

.upload-text {
	font-size: 32rpx;
	font-weight: 600;
}

.work-modal {
	position: fixed;
	top: 0;
	left: 0;
	right: 0;
	bottom: 0;
	background: rgba(0, 0, 0, 0.8);
	display: flex;
	align-items: center;
	justify-content: center;
	z-index: 2000;
}

.work-content {
	background: #ffffff;
	border-radius: 24rpx;
	margin: 32rpx;
	max-height: 80vh;
	overflow: hidden;
	box-shadow: 0 16rpx 48rpx rgba(0, 0, 0, 0.3);
}

.work-header {
	display: flex;
	justify-content: space-between;
	align-items: center;
	padding: 32rpx;
	border-bottom: 1px solid #ecf0f1;
}

.work-title {
	font-size: 32rpx;
	font-weight: 600;
	color: #2c3e50;
	flex: 1;
	margin-right: 16rpx;
}

.work-close {
	font-size: 48rpx;
	color: #7f8c8d;
	line-height: 1;
}

.work-body {
	padding: 32rpx;
	max-height: 60vh;
	overflow-y: auto;
}

.work-image {
	width: 100%;
	height: 300rpx;
	background: #f8f9fa;
	border-radius: 16rpx;
	margin-bottom: 24rpx;
}

.work-description {
	margin-bottom: 24rpx;
}

.work-desc-text {
	font-size: 28rpx;
	color: #2c3e50;
	line-height: 1.6;
}

.work-ai-quote {
	background: #f8f9fa;
	padding: 24rpx;
	border-radius: 16rpx;
	margin-bottom: 32rpx;
	border-left: 4rpx solid #3498db;
}

.ai-quote-label {
	display: block;
	font-size: 24rpx;
	color: #7f8c8d;
	margin-bottom: 8rpx;
}

.ai-quote-text {
	font-size: 26rpx;
	color: #2c3e50;
	line-height: 1.6;
	font-style: italic;
}

.work-actions {
	display: flex;
	gap: 24rpx;
}

.action-btn {
	flex: 1;
	height: 72rpx;
	border-radius: 36rpx;
	border: none;
	font-size: 28rpx;
	font-weight: 600;
	display: flex;
	align-items: center;
	justify-content: center;
	transition: all 0.2s ease;
}

.action-btn.like {
	background: #ffffff;
	color: #e74c3c;
	border: 2rpx solid #e74c3c;
}

.action-btn.share {
	background: linear-gradient(135deg, #3498db, #2980b9);
	color: #ffffff;
}

.action-btn:active {
	transform: scale(0.98);
}

.btn-text {
	font-size: 28rpx;
	font-weight: 600;
}
</style>
