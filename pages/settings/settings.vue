<template>
	<view class="settings-container">
		<!-- 自定义导航栏 -->
		<view class="custom-navbar">
			<view class="navbar-content">
				<text class="navbar-title">系统设置</text>
				<text class="navbar-back" @click="goBack">← 返回</text>
			</view>
		</view>
		
		<!-- 主体内容 -->
		<scroll-view class="content" scroll-y>
			<!-- 主题设置 -->
			<view class="settings-section">
				<text class="section-title">外观设置</text>
				<view class="setting-item">
					<view class="setting-info">
						<text class="setting-label">主题模式</text>
						<text class="setting-desc">选择浅色或深色主题</text>
					</view>
					<view class="theme-selector">
						<view 
							class="theme-option" 
							:class="{ 'theme-option-active': themeMode === 'light' }"
							@click="setTheme('light')"
						>
							<text class="theme-icon">☀️</text>
							<text class="theme-name">浅色</text>
						</view>
						<view 
							class="theme-option" 
							:class="{ 'theme-option-active': themeMode === 'dark' }"
							@click="setTheme('dark')"
						>
							<text class="theme-icon">🌙</text>
							<text class="theme-name">深色</text>
						</view>
					</view>
				</view>
			</view>
			
			<!-- 通知设置 -->
			<view class="settings-section">
				<text class="section-title">通知设置</text>
				<view class="setting-item">
					<view class="setting-info">
						<text class="setting-label">学习提醒</text>
						<text class="setting-desc">每日学习进度提醒</text>
					</view>
					<switch 
						:checked="notifications.learning" 
						@change="toggleNotification('learning')"
						color="#3498db"
					/>
				</view>
				<view class="setting-item">
					<view class="setting-info">
						<text class="setting-label">作品点赞</text>
						<text class="setting-desc">作品收到点赞时通知</text>
					</view>
					<switch 
						:checked="notifications.likes" 
						@change="toggleNotification('likes')"
						color="#3498db"
					/>
				</view>
				<view class="setting-item">
					<view class="setting-info">
						<text class="setting-label">系统更新</text>
						<text class="setting-desc">新功能发布通知</text>
					</view>
					<switch 
						:checked="notifications.updates" 
						@change="toggleNotification('updates')"
						color="#3498db"
					/>
				</view>
			</view>
			
			<!-- 数据管理 -->
			<view class="settings-section">
				<text class="section-title">数据管理</text>
				<view class="setting-item">
					<view class="setting-info">
						<text class="setting-label">清除缓存</text>
						<text class="setting-desc">清理应用缓存数据</text>
					</view>
					<button class="action-btn secondary" @click="clearCache">
						<text class="btn-text">清理</text>
					</button>
				</view>
				<view class="setting-item">
					<view class="setting-info">
						<text class="setting-label">导出数据</text>
						<text class="setting-desc">导出学习记录和作品</text>
					</view>
					<button class="action-btn secondary" @click="exportData">
						<text class="btn-text">导出</text>
					</button>
				</view>
			</view>
			
			<!-- 账户设置 -->
			<view class="settings-section">
				<text class="section-title">账户设置</text>
				<view class="setting-item">
					<view class="setting-info">
						<text class="setting-label">退出登录</text>
						<text class="setting-desc">安全退出当前账户</text>
					</view>
					<button class="action-btn danger" @click="logout">
						<text class="btn-text">退出</text>
					</button>
				</view>
			</view>
			
			<!-- 关于应用 -->
			<view class="settings-section">
				<text class="section-title">关于AI血缘</text>
				<view class="about-card">
					<view class="app-info">
						<text class="app-name">AI血缘</text>
						<text class="app-version">版本 1.0.0</text>
					</view>
					<text class="app-desc">一个专注于AI学习与创作的平台，让每个人都能在人工智能的世界中找到自己的位置。</text>
					<view class="app-features">
						<text class="feature-item">✨ 智能学习路径规划</text>
						<text class="feature-item">🎨 AI作品创作工具</text>
						<text class="feature-item">📊 学习进度可视化</text>
						<text class="feature-item">🤝 社区交流互动</text>
					</view>
					<button class="about-btn" @click="showAbout">
						<text class="btn-text">了解更多</text>
					</button>
				</view>
			</view>
		</scroll-view>
		
		<!-- 关于弹窗 -->
		<view class="about-modal" v-if="showAboutModal" @click="closeAbout">
			<view class="about-content" @click.stop>
				<view class="about-header">
					<text class="about-title">关于AI血缘</text>
					<text class="about-close" @click="closeAbout">×</text>
				</view>
				<view class="about-body">
					<text class="about-text">AI血缘是一个创新的AI学习平台，致力于帮助用户建立完整的AI知识体系。</text>
					<text class="about-text">我们相信，通过系统化的学习和实践，每个人都能成为AI领域的专家。</text>
					<view class="about-team">
						<text class="team-title">开发团队</text>
						<text class="team-member">产品经理：张三</text>
						<text class="team-member">技术总监：李四</text>
						<text class="team-member">UI设计师：王五</text>
					</view>
					<text class="about-contact">联系我们：contact@example.invalid</text>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			themeMode: 'light',
			notifications: {
				learning: true,
				likes: true,
				updates: false
			},
			showAboutModal: false
		}
	},
	onLoad() {
		this.loadSettings()
	},
	methods: {
		loadSettings() {
			// 加载用户设置
			uni.getStorage({
				key: 'userSettings',
				success: (res) => {
					if (res.data) {
						this.themeMode = res.data.themeMode || 'light'
						this.notifications = { ...this.notifications, ...res.data.notifications }
					}
				}
			})
		},
		saveSettings() {
			// 保存用户设置
			const settings = {
				themeMode: this.themeMode,
				notifications: this.notifications
			}
			uni.setStorage({
				key: 'userSettings',
				data: settings
			})
		},
		setTheme(mode) {
			this.themeMode = mode
			this.saveSettings()
			
			uni.showToast({
				title: `已切换到${mode === 'light' ? '浅色' : '深色'}主题`,
				icon: 'success'
			})
		},
		toggleNotification(type) {
			this.notifications[type] = !this.notifications[type]
			this.saveSettings()
		},
		clearCache() {
			uni.showModal({
				title: '确认清理',
				content: '确定要清理应用缓存吗？这将删除所有临时数据。',
				success: (res) => {
					if (res.confirm) {
						// 模拟清理缓存
						uni.showLoading({
							title: '清理中...'
						})
						
						setTimeout(() => {
							uni.hideLoading()
							uni.showToast({
								title: '缓存清理完成',
								icon: 'success'
							})
						}, 1500)
					}
				}
			})
		},
		exportData() {
			uni.showLoading({
				title: '导出中...'
			})
			
			// 模拟导出数据
			setTimeout(() => {
				uni.hideLoading()
				uni.showToast({
					title: '数据导出成功',
					icon: 'success'
				})
			}, 2000)
		},
		logout() {
			uni.showModal({
				title: '确认退出',
				content: '确定要退出登录吗？',
				success: (res) => {
					if (res.confirm) {
						// 清除用户数据
						uni.clearStorage()
						
						uni.showToast({
							title: '已退出登录',
							icon: 'success'
						})
						
						// 返回登录页面或首页
						setTimeout(() => {
							uni.reLaunch({
								url: '/pages/home/home'
							})
						}, 1500)
					}
				}
			})
		},
		showAbout() {
			this.showAboutModal = true
		},
		closeAbout() {
			this.showAboutModal = false
		},
		goBack() {
			uni.navigateBack()
		}
	}
}
</script>

<style scoped>
.settings-container {
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
	justify-content: space-between;
	padding: 0 32rpx;
	padding-top: var(--status-bar-height);
}

.navbar-title {
	font-size: 36rpx;
	font-weight: 600;
	color: #2c3e50;
}

.navbar-back {
	font-size: 32rpx;
	color: #3498db;
}

.content {
	margin-top: 88rpx;
	padding: 32rpx;
	padding-bottom: 120rpx;
}

.settings-section {
	margin-bottom: 40rpx;
}

.section-title {
	display: block;
	font-size: 32rpx;
	font-weight: 600;
	color: #2c3e50;
	margin-bottom: 24rpx;
}

.setting-item {
	background: #ffffff;
	border-radius: 16rpx;
	padding: 32rpx;
	margin-bottom: 16rpx;
	box-shadow: 0 4rpx 16rpx rgba(0, 0, 0, 0.08);
	border: 1px solid rgba(0, 0, 0, 0.05);
	display: flex;
	justify-content: space-between;
	align-items: center;
}

.setting-info {
	flex: 1;
}

.setting-label {
	display: block;
	font-size: 28rpx;
	font-weight: 600;
	color: #2c3e50;
	margin-bottom: 8rpx;
}

.setting-desc {
	font-size: 24rpx;
	color: #7f8c8d;
	line-height: 1.4;
}

.theme-selector {
	display: flex;
	gap: 16rpx;
}

.theme-option {
	display: flex;
	flex-direction: column;
	align-items: center;
	padding: 20rpx 24rpx;
	background: #f8f9fa;
	border-radius: 16rpx;
	border: 2rpx solid #ecf0f1;
	transition: all 0.2s ease;
}

.theme-option-active {
	background: #3498db;
	border-color: #2980b9;
}

.theme-option-active .theme-name {
	color: #ffffff;
}

.theme-icon {
	font-size: 32rpx;
	margin-bottom: 8rpx;
}

.theme-name {
	font-size: 22rpx;
	color: #2c3e50;
	font-weight: 600;
}

.action-btn {
	height: 64rpx;
	padding: 0 24rpx;
	border-radius: 32rpx;
	border: none;
	font-size: 24rpx;
	font-weight: 600;
	display: flex;
	align-items: center;
	justify-content: center;
	transition: all 0.2s ease;
}

.action-btn.secondary {
	background: #ecf0f1;
	color: #2c3e50;
}

.action-btn.danger {
	background: #e74c3c;
	color: #ffffff;
}

.action-btn:active {
	transform: scale(0.98);
}

.btn-text {
	font-size: 24rpx;
	font-weight: 600;
}

.about-card {
	background: #ffffff;
	border-radius: 20rpx;
	padding: 32rpx;
	box-shadow: 0 8rpx 32rpx rgba(0, 0, 0, 0.08);
	border: 1px solid rgba(0, 0, 0, 0.05);
}

.app-info {
	text-align: center;
	margin-bottom: 24rpx;
}

.app-name {
	display: block;
	font-size: 36rpx;
	font-weight: 700;
	color: #2c3e50;
	margin-bottom: 8rpx;
}

.app-version {
	font-size: 24rpx;
	color: #7f8c8d;
}

.app-desc {
	font-size: 26rpx;
	color: #5a6c7d;
	line-height: 1.6;
	margin-bottom: 24rpx;
}

.app-features {
	margin-bottom: 32rpx;
}

.feature-item {
	display: block;
	font-size: 24rpx;
	color: #2c3e50;
	margin-bottom: 12rpx;
	line-height: 1.5;
}

.about-btn {
	width: 100%;
	height: 72rpx;
	background: linear-gradient(135deg, #3498db, #2980b9);
	color: #ffffff;
	border: none;
	border-radius: 36rpx;
	font-size: 28rpx;
	font-weight: 600;
	display: flex;
	align-items: center;
	justify-content: center;
	box-shadow: 0 8rpx 24rpx rgba(52, 152, 219, 0.3);
	transition: all 0.2s ease;
}

.about-btn:active {
	transform: scale(0.98);
}

.about-modal {
	position: fixed;
	top: 0;
	left: 0;
	right: 0;
	bottom: 0;
	background: rgba(0, 0, 0, 0.5);
	display: flex;
	align-items: center;
	justify-content: center;
	z-index: 2000;
}

.about-content {
	background: #ffffff;
	border-radius: 24rpx;
	margin: 32rpx;
	max-height: 80vh;
	overflow: hidden;
	box-shadow: 0 16rpx 48rpx rgba(0, 0, 0, 0.2);
}

.about-header {
	display: flex;
	justify-content: space-between;
	align-items: center;
	padding: 32rpx;
	border-bottom: 1px solid #ecf0f1;
}

.about-title {
	font-size: 36rpx;
	font-weight: 600;
	color: #2c3e50;
}

.about-close {
	font-size: 48rpx;
	color: #7f8c8d;
	line-height: 1;
}

.about-body {
	padding: 32rpx;
	max-height: 60vh;
	overflow-y: auto;
}

.about-text {
	display: block;
	font-size: 28rpx;
	color: #2c3e50;
	line-height: 1.6;
	margin-bottom: 24rpx;
}

.about-team {
	margin: 32rpx 0;
	padding: 24rpx;
	background: #f8f9fa;
	border-radius: 16rpx;
}

.team-title {
	display: block;
	font-size: 28rpx;
	font-weight: 600;
	color: #2c3e50;
	margin-bottom: 16rpx;
}

.team-member {
	display: block;
	font-size: 24rpx;
	color: #5a6c7d;
	margin-bottom: 8rpx;
}

.about-contact {
	font-size: 24rpx;
	color: #3498db;
	text-align: center;
}
</style>
