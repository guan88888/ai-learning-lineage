<template>
	<view class="home-container">
		<!-- 自定义导航栏 -->
		<view class="custom-navbar">
			<view class="navbar-content">
				<text class="navbar-title">AI血缘 - 数据舱</text>
			</view>
		</view>
		
		<!-- 主体内容 -->
		<scroll-view class="content" scroll-y>
			<!-- 欢迎语区域 -->
			<view class="welcome-section">
				<view class="welcome-card">
					<text class="welcome-text">你好，{{ userName }}！</text>
					<text class="welcome-subtitle">今天也要和AI一起成长哦 ✨</text>
				</view>
			</view>
			
			<!-- 今日学习进度 -->
			<view class="progress-section">
				<view class="progress-card">
					<view class="progress-header">
						<text class="progress-title">今日学习进度</text>
						<text class="progress-percent">{{ todayProgress }}%</text>
					</view>
					<view class="progress-bar">
						<view class="progress-fill" :style="{ width: todayProgress + '%' }"></view>
					</view>
					<text class="progress-desc">已完成 {{ completedTasks }}/{{ totalTasks }} 个学习任务</text>
				</view>
			</view>
			
			<!-- 最近项目卡片 -->
			<view class="projects-section">
				<view class="section-header">
					<text class="section-title">最近项目</text>
					<text class="section-more" @click="goToArchive">查看全部</text>
				</view>
				<view class="projects-grid">
					<view 
						class="project-card" 
						v-for="(project, index) in recentProjects" 
						:key="index"
						@click="viewProject(project)"
					>
						<image class="project-cover" :src="project.cover" mode="aspectFill"></image>
						<view class="project-info">
							<text class="project-title">{{ project.title }}</text>
							<text class="project-time">{{ project.updateTime }}</text>
						</view>
					</view>
				</view>
			</view>
			
			<!-- 操作按钮 -->
			<view class="action-buttons">
				<button class="action-btn primary" @click="goToArchive">
					<text class="btn-text">进入学习档案</text>
				</button>
				<button class="action-btn secondary" @click="goToGallery">
					<text class="btn-text">生成新作品</text>
				</button>
			</view>
		</scroll-view>
		
		<!-- 自定义底部导航栏 -->
		<custom-tabbar :current="0" @change="onTabChange"></custom-tabbar>
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
			userName: 'AI学习者',
			todayProgress: 75,
			completedTasks: 3,
			totalTasks: 4,
			recentProjects: [
				{
					id: 1,
					title: '机器学习基础',
					cover: '/static/project1.jpg',
					updateTime: '2小时前'
				},
				{
					id: 2,
					title: '深度学习实践',
					cover: '/static/project2.jpg',
					updateTime: '1天前'
				},
				{
					id: 3,
					title: '自然语言处理',
					cover: '/static/project3.jpg',
					updateTime: '3天前'
				}
			]
		}
	},
	onLoad() {
		this.loadUserData()
	},
	methods: {
		loadUserData() {
			// 模拟加载用户数据
			uni.getStorage({
				key: 'userName',
				success: (res) => {
					this.userName = res.data || 'AI学习者'
				}
			})
		},
		goToArchive() {
			uni.switchTab({
				url: '/pages/archive/archive'
			})
		},
		goToGallery() {
			uni.switchTab({
				url: '/pages/gallery/gallery'
			})
		},
		viewProject(project) {
			uni.showToast({
				title: `查看项目：${project.title}`,
				icon: 'none'
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
.home-container {
	min-height: 100vh;
	background: linear-gradient(135deg, #00CED1 0%, #40E0D0 20%, #00CED1 40%, #1E90FF 60%, #0000CD 80%, #00CED1 100%);
	position: relative;
	overflow: hidden;
}

.home-container::before {
	content: '';
	position: fixed;
	top: 0;
	left: 0;
	right: 0;
	bottom: 0;
	background: 
		radial-gradient(ellipse at 20% 30%, rgba(0, 206, 209, 0.3) 0%, transparent 50%),
		radial-gradient(ellipse at 80% 70%, rgba(30, 144, 255, 0.3) 0%, transparent 50%),
		radial-gradient(ellipse at 50% 50%, rgba(64, 224, 208, 0.2) 0%, transparent 60%);
	z-index: -1;
	pointer-events: none;
}

.custom-navbar {
	position: fixed;
	top: 0;
	left: 0;
	right: 0;
	z-index: 1000;
	background: rgba(255, 255, 255, 0.95);
	backdrop-filter: blur(20rpx);
	border-bottom: 1rpx solid rgba(0, 206, 209, 0.3);
	box-shadow: 0 2rpx 20rpx rgba(0, 206, 209, 0.15);
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
	font-weight: 700;
	color: #00CED1;
}

.content {
	margin-top: 88rpx;
	padding: 32rpx;
	padding-bottom: 130rpx;
}

.welcome-section {
	margin-bottom: 40rpx;
}

.welcome-card {
	background: rgba(255, 255, 255, 0.95);
	border-radius: 8rpx;
	padding: 40rpx;
	box-shadow: 0 4rpx 20rpx rgba(0, 206, 209, 0.2);
	border: 2rpx solid rgba(0, 206, 209, 0.3);
	position: relative;
	overflow: hidden;
	backdrop-filter: blur(10rpx);
}

.welcome-text {
	display: block;
	font-size: 48rpx;
	font-weight: 700;
	color: #00CED1;
	margin-bottom: 16rpx;
}

.welcome-subtitle {
	font-size: 28rpx;
	color: #535c68;
	line-height: 1.5;
}

.progress-section {
	margin-bottom: 40rpx;
}

.progress-card {
	background: rgba(255, 255, 255, 0.95);
	border-radius: 8rpx;
	padding: 32rpx;
	box-shadow: 0 4rpx 20rpx rgba(0, 206, 209, 0.2);
	border: 2rpx solid rgba(0, 206, 209, 0.3);
	backdrop-filter: blur(10rpx);
}

.progress-header {
	display: flex;
	justify-content: space-between;
	align-items: center;
	margin-bottom: 24rpx;
}

.progress-title {
	font-size: 32rpx;
	font-weight: 600;
	color: #2c3e50;
}

.progress-percent {
	font-size: 36rpx;
	font-weight: 700;
	color: #3498db;
}

.progress-bar {
	height: 12rpx;
	background: #ecf0f1;
	border-radius: 6rpx;
	overflow: hidden;
	margin-bottom: 16rpx;
}

.progress-fill {
	height: 100%;
	background: linear-gradient(90deg, #3498db, #2980b9);
	border-radius: 6rpx;
	transition: width 0.3s ease;
}

.progress-desc {
	font-size: 24rpx;
	color: #7f8c8d;
}

.projects-section {
	margin-bottom: 40rpx;
}

.section-header {
	display: flex;
	justify-content: space-between;
	align-items: center;
	margin-bottom: 24rpx;
}

.section-title {
	font-size: 32rpx;
	font-weight: 600;
	color: #2c3e50;
}

.section-more {
	font-size: 24rpx;
	color: #3498db;
}

.projects-grid {
	display: flex;
	flex-direction: column;
	gap: 24rpx;
}

.project-card {
	background: rgba(255, 255, 255, 0.95);
	border-radius: 8rpx;
	overflow: hidden;
	box-shadow: 0 4rpx 20rpx rgba(0, 206, 209, 0.2);
	border: 2rpx solid rgba(0, 206, 209, 0.3);
	backdrop-filter: blur(10rpx);
	transition: transform 0.2s ease;
}

.project-card:active {
	transform: scale(0.98);
}

.project-cover {
	width: 100%;
	height: 200rpx;
	background: #f8f9fa;
}

.project-info {
	padding: 24rpx;
}

.project-title {
	display: block;
	font-size: 28rpx;
	font-weight: 600;
	color: #2c3e50;
	margin-bottom: 8rpx;
}

.project-time {
	font-size: 22rpx;
	color: #7f8c8d;
}

.action-buttons {
	display: flex;
	flex-direction: column;
	gap: 24rpx;
}

.action-btn {
	height: 88rpx;
	border-radius: 8rpx;
	border: none;
	font-size: 32rpx;
	font-weight: 700;
	display: flex;
	align-items: center;
	justify-content: center;
	transition: all 0.2s ease;
}

.action-btn.primary {
	background: linear-gradient(135deg, #00CED1, #1E90FF);
	color: #ffffff;
	box-shadow: 0 6rpx 20rpx rgba(0, 206, 209, 0.4);
	border: 2rpx solid rgba(0, 206, 209, 0.5);
}

.action-btn.secondary {
	background: rgba(255, 255, 255, 0.95);
	color: #00CED1;
	border: 2rpx solid #00CED1;
	box-shadow: 0 6rpx 20rpx rgba(0, 206, 209, 0.2);
	backdrop-filter: blur(10rpx);
}

.action-btn:active {
	transform: scale(0.98);
}

.btn-text {
	font-size: 32rpx;
	font-weight: 600;
}

</style>
