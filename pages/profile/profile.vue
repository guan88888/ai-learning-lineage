<template>
	<view class="profile-container">
		<!-- 自定义导航栏 -->
		<view class="custom-navbar">
			<view class="navbar-content">
				<text class="navbar-title">个人档案</text>
			</view>
		</view>
		
		<!-- 主体内容 -->
		<scroll-view class="content" scroll-y>
			<!-- 用户信息卡片 -->
			<view class="user-card">
				<view class="user-avatar" @click="goToAvatar">
					<image class="avatar-image" :src="userInfo.avatar" mode="aspectFill"></image>
					<view class="avatar-edit">
						<text class="edit-icon">✏️</text>
					</view>
				</view>
				<view class="user-info">
					<text class="user-name">{{ userInfo.name }}</text>
					<text class="user-title">{{ userInfo.title }}</text>
					<text class="user-desc">{{ userInfo.description }}</text>
				</view>
			</view>
			
			<!-- AI学习标签 -->
			<view class="skills-section">
				<view class="section-header">
					<text class="section-title">AI学习标签</text>
				</view>
				<view class="skills-tags">
					<text 
						class="skill-tag" 
						v-for="(skill, index) in userSkills" 
						:key="index"
						:class="{ 'skill-tag-active': skill.active }"
						@click="toggleSkill(index)"
					>{{ skill.name }}</text>
				</view>
			</view>
			
			<!-- 我的作品 -->
			<view class="my-works-section">
				<view class="section-header">
					<text class="section-title">我的作品</text>
					<text class="section-more" @click="goToGallery">查看全部</text>
				</view>
				<view class="works-grid">
					<view 
						class="work-item" 
						v-for="(work, index) in myWorks" 
						:key="index"
						@click="viewWork(work)"
					>
						<image class="work-cover" :src="work.cover" mode="aspectFill"></image>
						<view class="work-info">
							<text class="work-title">{{ work.title }}</text>
							<text class="work-likes">❤️ {{ work.likes }}</text>
						</view>
					</view>
				</view>
			</view>
			
			<!-- 学习统计 -->
			<view class="stats-section">
				<view class="section-header">
					<text class="section-title">学习统计</text>
				</view>
				<view class="stats-grid">
					<view class="stat-item">
						<text class="stat-number">{{ userStats.totalDays }}</text>
						<text class="stat-label">学习天数</text>
					</view>
					<view class="stat-item">
						<text class="stat-number">{{ userStats.totalHours }}</text>
						<text class="stat-label">学习时长</text>
					</view>
					<view class="stat-item">
						<text class="stat-number">{{ userStats.completedProjects }}</text>
						<text class="stat-label">完成项目</text>
					</view>
					<view class="stat-item">
						<text class="stat-number">{{ userStats.totalWorks }}</text>
						<text class="stat-label">创作作品</text>
					</view>
				</view>
			</view>
			
			<!-- 设置按钮 -->
			<view class="settings-section">
				<button class="settings-btn" @click="goToSettings">
					<text class="settings-text">⚙️ 系统设置</text>
				</button>
			</view>
		</scroll-view>
		
		<!-- 自定义底部导航栏 -->
		<custom-tabbar :current="3" @change="onTabChange"></custom-tabbar>
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
			userInfo: {
				name: 'AI学习者',
				title: '机器学习工程师',
				description: '热爱AI技术，专注于深度学习与自然语言处理',
				avatar: '/static/avatar.jpg'
			},
			userSkills: [
				{ name: 'Python', active: true },
				{ name: '机器学习', active: true },
				{ name: '深度学习', active: true },
				{ name: '自然语言处理', active: false },
				{ name: '计算机视觉', active: true },
				{ name: 'TensorFlow', active: false },
				{ name: 'PyTorch', active: true },
				{ name: '数据分析', active: true }
			],
			userStats: {
				totalDays: 45,
				totalHours: 128,
				completedProjects: 12,
				totalWorks: 8
			},
			myWorks: [
				{
					id: 1,
					title: 'AI绘画作品',
					cover: '/static/mywork1.jpg',
					likes: 128
				},
				{
					id: 2,
					title: '机器学习模型',
					cover: '/static/mywork2.jpg',
					likes: 95
				},
				{
					id: 3,
					title: '深度学习项目',
					cover: '/static/mywork3.jpg',
					likes: 76
				},
				{
					id: 4,
					title: 'NLP应用',
					cover: '/static/mywork4.jpg',
					likes: 142
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
				key: 'userInfo',
				success: (res) => {
					if (res.data) {
						this.userInfo = { ...this.userInfo, ...res.data }
					}
				}
			})
		},
		goToAvatar() {
			uni.navigateTo({
				url: '/pages/avatar/avatar'
			})
		},
		goToGallery() {
			uni.switchTab({
				url: '/pages/gallery/gallery'
			})
		},
		goToSettings() {
			uni.navigateTo({
				url: '/pages/settings/settings'
			})
		},
		toggleSkill(index) {
			this.userSkills[index].active = !this.userSkills[index].active
		},
		viewWork(work) {
			uni.showToast({
				title: `查看作品：${work.title}`,
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
.profile-container {
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

.user-card {
	background: #ffffff;
	border-radius: 24rpx;
	padding: 40rpx;
	box-shadow: 0 8rpx 32rpx rgba(0, 0, 0, 0.08);
	border: 1px solid rgba(0, 0, 0, 0.05);
	margin-bottom: 32rpx;
	display: flex;
	align-items: center;
}

.user-avatar {
	position: relative;
	margin-right: 32rpx;
}

.avatar-image {
	width: 120rpx;
	height: 120rpx;
	border-radius: 60rpx;
	background: #f8f9fa;
}

.avatar-edit {
	position: absolute;
	bottom: 0;
	right: 0;
	width: 36rpx;
	height: 36rpx;
	background: #3498db;
	border-radius: 18rpx;
	display: flex;
	align-items: center;
	justify-content: center;
	border: 3rpx solid #ffffff;
}

.edit-icon {
	font-size: 20rpx;
	color: #ffffff;
}

.user-info {
	flex: 1;
}

.user-name {
	display: block;
	font-size: 36rpx;
	font-weight: 700;
	color: #2c3e50;
	margin-bottom: 8rpx;
}

.user-title {
	display: block;
	font-size: 24rpx;
	color: #3498db;
	margin-bottom: 12rpx;
}

.user-desc {
	font-size: 26rpx;
	color: #7f8c8d;
	line-height: 1.5;
}

.skills-section {
	margin-bottom: 32rpx;
}

.section-header {
	margin-bottom: 24rpx;
}

.section-title {
	font-size: 32rpx;
	font-weight: 600;
	color: #2c3e50;
}

.skills-tags {
	display: flex;
	flex-wrap: wrap;
	gap: 16rpx;
}

.skill-tag {
	padding: 12rpx 24rpx;
	background: #ecf0f1;
	color: #7f8c8d;
	font-size: 24rpx;
	border-radius: 24rpx;
	transition: all 0.2s ease;
}

.skill-tag-active {
	background: #3498db;
	color: #ffffff;
}

.my-works-section {
	margin-bottom: 32rpx;
}

.section-header {
	display: flex;
	justify-content: space-between;
	align-items: center;
	margin-bottom: 24rpx;
}

.section-more {
	font-size: 24rpx;
	color: #3498db;
}

.works-grid {
	display: grid;
	grid-template-columns: 1fr 1fr;
	gap: 20rpx;
}

.work-item {
	background: #ffffff;
	border-radius: 16rpx;
	overflow: hidden;
	box-shadow: 0 4rpx 16rpx rgba(0, 0, 0, 0.08);
	border: 1px solid rgba(0, 0, 0, 0.05);
	transition: transform 0.2s ease;
}

.work-item:active {
	transform: scale(0.98);
}

.work-cover {
	width: 100%;
	height: 160rpx;
	background: #f8f9fa;
}

.work-info {
	padding: 16rpx;
}

.work-title {
	display: block;
	font-size: 22rpx;
	font-weight: 600;
	color: #2c3e50;
	margin-bottom: 8rpx;
	overflow: hidden;
	text-overflow: ellipsis;
	white-space: nowrap;
}

.work-likes {
	font-size: 20rpx;
	color: #e74c3c;
}

.stats-section {
	margin-bottom: 32rpx;
}

.stats-grid {
	display: grid;
	grid-template-columns: 1fr 1fr;
	gap: 20rpx;
}

.stat-item {
	background: #ffffff;
	border-radius: 16rpx;
	padding: 24rpx;
	text-align: center;
	box-shadow: 0 4rpx 16rpx rgba(0, 0, 0, 0.08);
	border: 1px solid rgba(0, 0, 0, 0.05);
}

.stat-number {
	display: block;
	font-size: 36rpx;
	font-weight: 700;
	color: #3498db;
	margin-bottom: 8rpx;
}

.stat-label {
	font-size: 22rpx;
	color: #7f8c8d;
}

.settings-section {
	display: flex;
	justify-content: center;
}

.settings-btn {
	width: 100%;
	height: 88rpx;
	background: #ffffff;
	color: #2c3e50;
	border: 2rpx solid #ecf0f1;
	border-radius: 44rpx;
	font-size: 32rpx;
	font-weight: 600;
	display: flex;
	align-items: center;
	justify-content: center;
	box-shadow: 0 8rpx 24rpx rgba(0, 0, 0, 0.08);
	transition: all 0.2s ease;
}

.settings-btn:active {
	transform: scale(0.98);
}

.settings-text {
	font-size: 32rpx;
	font-weight: 600;
}
</style>
