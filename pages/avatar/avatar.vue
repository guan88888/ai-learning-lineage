<template>
	<view class="avatar-container">
		<!-- 自定义导航栏 -->
		<view class="custom-navbar">
			<view class="navbar-content">
				<text class="navbar-title">AI形象生成</text>
				<text class="navbar-back" @click="goBack">← 返回</text>
			</view>
		</view>
		
		<!-- 主体内容 -->
		<scroll-view class="content" scroll-y>
			<!-- 当前形象展示 -->
			<view class="avatar-display">
				<view class="avatar-card">
					<text class="avatar-title">当前AI形象</text>
					<view class="avatar-preview" @click="previewAvatar">
						<image 
							class="avatar-image" 
							:src="currentAvatar" 
							mode="aspectFill"
						/>
						<view class="avatar-overlay">
							<text class="preview-text">点击预览</text>
						</view>
					</view>
					<text class="avatar-desc">{{ currentAvatarDesc }}</text>
				</view>
			</view>
			
			<!-- 形象生成选项 -->
			<view class="generation-section">
				<text class="section-title">生成新形象</text>
				
				<!-- 个性关键词 -->
				<view class="form-group">
					<text class="form-label">个性关键词</text>
					<input 
						class="form-input" 
						placeholder="如：科技感、未来风、极简主义..." 
						v-model="generationData.keywords"
					/>
				</view>
				
				<!-- 颜色偏好 -->
				<view class="form-group">
					<text class="form-label">颜色偏好</text>
					<view class="color-selector">
						<view 
							class="color-option" 
							v-for="(color, index) in colorOptions" 
							:key="index"
							:class="{ 'color-option-active': generationData.color === color.value }"
							:style="{ backgroundColor: color.value }"
							@click="selectColor(color.value)"
						></view>
					</view>
				</view>
				
				<!-- 风格选择 -->
				<view class="form-group">
					<text class="form-label">风格选择</text>
					<view class="style-selector">
						<view 
							class="style-option" 
							v-for="(style, index) in styleOptions" 
							:key="index"
							:class="{ 'style-option-active': generationData.style === style.value }"
							@click="selectStyle(style.value)"
						>
							<text class="style-icon">{{ style.icon }}</text>
							<text class="style-name">{{ style.name }}</text>
						</view>
					</view>
				</view>
				
				<!-- 生成按钮 -->
				<button class="generate-btn" @click="generateAvatar" :disabled="isGenerating">
					<text class="btn-text">{{ isGenerating ? '生成中...' : '🤖 生成新形象' }}</text>
				</button>
			</view>
			
			<!-- 历史形象 -->
			<view class="history-section">
				<text class="section-title">历史形象</text>
				<view class="history-grid">
					<view 
						class="history-item" 
						v-for="(avatar, index) in avatarHistory" 
						:key="index"
						@click="selectHistoryAvatar(avatar)"
					>
						<image class="history-image" :src="avatar.image" mode="aspectFill"></image>
						<text class="history-desc">{{ avatar.desc }}</text>
					</view>
				</view>
			</view>
			
			<!-- 保存按钮 -->
			<view class="save-section">
				<button class="save-btn" @click="saveAvatar">
					<text class="btn-text">💾 保存当前形象</text>
				</button>
			</view>
		</scroll-view>
		
		<!-- 形象预览弹窗 -->
		<view class="preview-modal" v-if="showPreview" @click="closePreview">
			<view class="preview-content" @click.stop>
				<image class="preview-image" :src="currentAvatar" mode="aspectFit"></image>
				<button class="preview-close" @click="closePreview">关闭</button>
			</view>
		</view>
		
		<!-- 生成进度 -->
		<view class="generating-modal" v-if="isGenerating">
			<view class="generating-content">
				<view class="generating-animation">
					<text class="generating-icon">🤖</text>
				</view>
				<text class="generating-text">AI正在为您生成专属形象...</text>
				<text class="generating-progress">{{ generatingProgress }}%</text>
			</view>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			currentAvatar: '/static/avatar-default.jpg',
			currentAvatarDesc: '您的专属AI形象，展现独特的数字身份',
			showPreview: false,
			isGenerating: false,
			generatingProgress: 0,
			generationData: {
				keywords: '',
				color: '#3498db',
				style: 'cyber'
			},
			colorOptions: [
				{ value: '#3498db', name: '科技蓝' },
				{ value: '#e74c3c', name: '活力红' },
				{ value: '#2ecc71', name: '自然绿' },
				{ value: '#f39c12', name: '阳光橙' },
				{ value: '#9b59b6', name: '神秘紫' },
				{ value: '#34495e', name: '经典灰' }
			],
			styleOptions: [
				{ value: 'cyber', name: '赛博朋克', icon: '🤖' },
				{ value: 'minimal', name: '极简主义', icon: '⚪' },
				{ value: 'futuristic', name: '未来科技', icon: '🚀' },
				{ value: 'artistic', name: '艺术风格', icon: '🎨' },
				{ value: 'geometric', name: '几何图形', icon: '🔷' },
				{ value: 'organic', name: '有机形态', icon: '🌿' }
			],
			avatarHistory: [
				{
					image: '/static/avatar1.jpg',
					desc: '科技感十足'
				},
				{
					image: '/static/avatar2.jpg',
					desc: '极简风格'
				},
				{
					image: '/static/avatar3.jpg',
					desc: '未来主义'
				},
				{
					image: '/static/avatar4.jpg',
					desc: '艺术创作'
				}
			]
		}
	},
	onLoad() {
		this.loadCurrentAvatar()
	},
	methods: {
		loadCurrentAvatar() {
			// 加载当前用户头像
			uni.getStorage({
				key: 'userAvatar',
				success: (res) => {
					if (res.data) {
						this.currentAvatar = res.data.image
						this.currentAvatarDesc = res.data.desc
					}
				}
			})
		},
		previewAvatar() {
			this.showPreview = true
		},
		closePreview() {
			this.showPreview = false
		},
		selectColor(color) {
			this.generationData.color = color
		},
		selectStyle(style) {
			this.generationData.style = style
		},
		generateAvatar() {
			if (!this.generationData.keywords) {
				uni.showToast({
					title: '请输入个性关键词',
					icon: 'none'
				})
				return
			}
			
			this.isGenerating = true
			this.generatingProgress = 0
			
			// 模拟生成进度
			const progressInterval = setInterval(() => {
				this.generatingProgress += Math.random() * 20
				if (this.generatingProgress >= 100) {
					this.generatingProgress = 100
					clearInterval(progressInterval)
					
					// 生成完成
					setTimeout(() => {
						this.generateNewAvatar()
					}, 500)
				}
			}, 200)
		},
		generateNewAvatar() {
			// 模拟AI生成新形象
			const newAvatar = {
				image: '/static/avatar-generated.jpg',
				desc: `基于"${this.generationData.keywords}"生成的${this.getStyleName(this.generationData.style)}形象`
			}
			
			this.currentAvatar = newAvatar.image
			this.currentAvatarDesc = newAvatar.desc
			
			// 添加到历史记录
			this.avatarHistory.unshift(newAvatar)
			if (this.avatarHistory.length > 6) {
				this.avatarHistory.pop()
			}
			
			this.isGenerating = false
			this.generatingProgress = 0
			
			uni.showToast({
				title: '形象生成成功！',
				icon: 'success'
			})
		},
		getStyleName(style) {
			const styleMap = {
				'cyber': '赛博朋克',
				'minimal': '极简主义',
				'futuristic': '未来科技',
				'artistic': '艺术风格',
				'geometric': '几何图形',
				'organic': '有机形态'
			}
			return styleMap[style] || '独特'
		},
		selectHistoryAvatar(avatar) {
			this.currentAvatar = avatar.image
			this.currentAvatarDesc = avatar.desc
		},
		saveAvatar() {
			// 保存当前形象
			const avatarData = {
				image: this.currentAvatar,
				desc: this.currentAvatarDesc,
				timestamp: Date.now()
			}
			
			uni.setStorage({
				key: 'userAvatar',
				data: avatarData,
				success: () => {
					uni.showToast({
						title: '形象保存成功！',
						icon: 'success'
					})
					
					// 返回个人中心
					setTimeout(() => {
						uni.navigateBack()
					}, 1500)
				}
			})
		},
		goBack() {
			uni.navigateBack()
		}
	}
}
</script>

<style scoped>
.avatar-container {
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

.avatar-display {
	margin-bottom: 40rpx;
}

.avatar-card {
	background: #ffffff;
	border-radius: 24rpx;
	padding: 32rpx;
	box-shadow: 0 8rpx 32rpx rgba(0, 0, 0, 0.08);
	border: 1px solid rgba(0, 0, 0, 0.05);
	text-align: center;
}

.avatar-title {
	display: block;
	font-size: 32rpx;
	font-weight: 600;
	color: #2c3e50;
	margin-bottom: 24rpx;
}

.avatar-preview {
	position: relative;
	width: 200rpx;
	height: 200rpx;
	margin: 0 auto 24rpx;
	border-radius: 100rpx;
	overflow: hidden;
	box-shadow: 0 8rpx 24rpx rgba(0, 0, 0, 0.1);
	transition: transform 0.2s ease;
}

.avatar-preview:active {
	transform: scale(0.95);
}

.avatar-image {
	width: 100%;
	height: 100%;
	background: #f8f9fa;
}

.avatar-overlay {
	position: absolute;
	top: 0;
	left: 0;
	right: 0;
	bottom: 0;
	background: rgba(0, 0, 0, 0.5);
	display: flex;
	align-items: center;
	justify-content: center;
	opacity: 0;
	transition: opacity 0.2s ease;
}

.avatar-preview:active .avatar-overlay {
	opacity: 1;
}

.preview-text {
	color: #ffffff;
	font-size: 24rpx;
}

.avatar-desc {
	font-size: 26rpx;
	color: #7f8c8d;
	line-height: 1.5;
}

.generation-section {
	margin-bottom: 40rpx;
}

.section-title {
	display: block;
	font-size: 32rpx;
	font-weight: 600;
	color: #2c3e50;
	margin-bottom: 24rpx;
}

.form-group {
	margin-bottom: 32rpx;
}

.form-label {
	display: block;
	font-size: 28rpx;
	font-weight: 600;
	color: #2c3e50;
	margin-bottom: 16rpx;
}

.form-input {
	width: 100%;
	height: 80rpx;
	padding: 0 24rpx;
	background: #f8f9fa;
	border-radius: 16rpx;
	border: 2rpx solid #ecf0f1;
	font-size: 28rpx;
	color: #2c3e50;
	transition: border-color 0.2s ease;
}

.form-input:focus {
	border-color: #3498db;
}

.color-selector {
	display: flex;
	gap: 16rpx;
	flex-wrap: wrap;
}

.color-option {
	width: 60rpx;
	height: 60rpx;
	border-radius: 30rpx;
	border: 3rpx solid #ffffff;
	box-shadow: 0 4rpx 12rpx rgba(0, 0, 0, 0.1);
	transition: transform 0.2s ease;
}

.color-option-active {
	transform: scale(1.2);
	border-color: #2c3e50;
}

.color-option:active {
	transform: scale(0.9);
}

.style-selector {
	display: grid;
	grid-template-columns: 1fr 1fr 1fr;
	gap: 16rpx;
}

.style-option {
	display: flex;
	flex-direction: column;
	align-items: center;
	padding: 24rpx 16rpx;
	background: #f8f9fa;
	border-radius: 16rpx;
	border: 2rpx solid #ecf0f1;
	transition: all 0.2s ease;
}

.style-option-active {
	background: #3498db;
	border-color: #2980b9;
}

.style-option-active .style-name {
	color: #ffffff;
}

.style-icon {
	font-size: 32rpx;
	margin-bottom: 8rpx;
}

.style-name {
	font-size: 22rpx;
	color: #2c3e50;
	font-weight: 600;
}

.generate-btn {
	width: 100%;
	height: 88rpx;
	background: linear-gradient(135deg, #9b59b6, #8e44ad);
	color: #ffffff;
	border: none;
	border-radius: 44rpx;
	font-size: 32rpx;
	font-weight: 600;
	display: flex;
	align-items: center;
	justify-content: center;
	box-shadow: 0 8rpx 24rpx rgba(155, 89, 182, 0.3);
	transition: all 0.2s ease;
}

.generate-btn:disabled {
	background: #bdc3c7;
	box-shadow: none;
}

.generate-btn:active:not(:disabled) {
	transform: scale(0.98);
}

.history-section {
	margin-bottom: 40rpx;
}

.history-grid {
	display: grid;
	grid-template-columns: 1fr 1fr 1fr;
	gap: 16rpx;
}

.history-item {
	background: #ffffff;
	border-radius: 16rpx;
	padding: 16rpx;
	box-shadow: 0 4rpx 16rpx rgba(0, 0, 0, 0.08);
	border: 1px solid rgba(0, 0, 0, 0.05);
	text-align: center;
	transition: transform 0.2s ease;
}

.history-item:active {
	transform: scale(0.95);
}

.history-image {
	width: 100%;
	height: 120rpx;
	border-radius: 12rpx;
	background: #f8f9fa;
	margin-bottom: 12rpx;
}

.history-desc {
	font-size: 20rpx;
	color: #7f8c8d;
	line-height: 1.4;
}

.save-section {
	display: flex;
	justify-content: center;
}

.save-btn {
	width: 100%;
	height: 88rpx;
	background: linear-gradient(135deg, #2ecc71, #27ae60);
	color: #ffffff;
	border: none;
	border-radius: 44rpx;
	font-size: 32rpx;
	font-weight: 600;
	display: flex;
	align-items: center;
	justify-content: center;
	box-shadow: 0 8rpx 24rpx rgba(46, 204, 113, 0.3);
	transition: all 0.2s ease;
}

.save-btn:active {
	transform: scale(0.98);
}

.btn-text {
	font-size: 32rpx;
	font-weight: 600;
}

.preview-modal {
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

.preview-content {
	background: #ffffff;
	border-radius: 24rpx;
	padding: 32rpx;
	text-align: center;
	box-shadow: 0 16rpx 48rpx rgba(0, 0, 0, 0.3);
}

.preview-image {
	width: 400rpx;
	height: 400rpx;
	border-radius: 20rpx;
	background: #f8f9fa;
	margin-bottom: 24rpx;
}

.preview-close {
	background: #3498db;
	color: #ffffff;
	border: none;
	border-radius: 24rpx;
	padding: 16rpx 32rpx;
	font-size: 28rpx;
}

.generating-modal {
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

.generating-content {
	background: #ffffff;
	border-radius: 24rpx;
	padding: 40rpx;
	text-align: center;
	box-shadow: 0 16rpx 48rpx rgba(0, 0, 0, 0.3);
}

.generating-animation {
	margin-bottom: 24rpx;
}

.generating-icon {
	font-size: 64rpx;
	animation: spin 1s linear infinite;
}

@keyframes spin {
	from { transform: rotate(0deg); }
	to { transform: rotate(360deg); }
}

.generating-text {
	display: block;
	font-size: 28rpx;
	color: #2c3e50;
	margin-bottom: 16rpx;
}

.generating-progress {
	font-size: 24rpx;
	color: #3498db;
	font-weight: 600;
}
</style>
