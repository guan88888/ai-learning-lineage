<template>
	<view class="upload-container">
		<!-- 自定义导航栏 -->
		<view class="custom-navbar">
			<view class="navbar-content">
				<text class="navbar-title">上传新作品</text>
				<text class="navbar-back" @click="goBack">← 返回</text>
			</view>
		</view>
		
		<!-- 主体内容 -->
		<scroll-view class="content" scroll-y>
			<!-- 上传表单 -->
			<view class="upload-form">
				<!-- 作品标题 -->
				<view class="form-group">
					<text class="form-label">作品标题</text>
					<input 
						class="form-input" 
						placeholder="请输入作品标题..." 
						v-model="formData.title"
					/>
				</view>
				
				<!-- 作品描述 -->
				<view class="form-group">
					<text class="form-label">作品描述</text>
					<textarea 
						class="form-textarea" 
						placeholder="请描述您的作品..." 
						v-model="formData.description"
					></textarea>
				</view>
				
				<!-- 图片上传 -->
				<view class="form-group">
					<text class="form-label">作品图片</text>
					<view class="upload-area" @click="chooseImage">
						<view class="upload-placeholder" v-if="!formData.image">
							<text class="upload-icon">📷</text>
							<text class="upload-text">点击上传图片</text>
						</view>
						<image 
							class="upload-preview" 
							v-else 
							:src="formData.image" 
							mode="aspectFill"
						/>
					</view>
				</view>
				
				<!-- 作品类型 -->
				<view class="form-group">
					<text class="form-label">作品类型</text>
					<view class="type-selector">
						<view 
							class="type-option" 
							v-for="(type, index) in workTypes" 
							:key="index"
							:class="{ 'type-option-active': formData.type === type.value }"
							@click="selectType(type.value)"
						>
							<text class="type-icon">{{ type.icon }}</text>
							<text class="type-name">{{ type.name }}</text>
						</view>
					</view>
				</view>
				
				<!-- AI简介生成 -->
				<view class="form-group">
					<button class="ai-generate-btn" @click="generateAIDescription">
						<text class="btn-text">🤖 生成AI简介</text>
					</button>
					<view class="ai-description" v-if="aiDescription">
						<text class="ai-desc-label">AI生成的简介：</text>
						<text class="ai-desc-text">{{ aiDescription }}</text>
					</view>
				</view>
				
				<!-- 提交按钮 -->
				<view class="submit-section">
					<button class="submit-btn" @click="submitWork" :disabled="!canSubmit">
						<text class="btn-text">发布作品</text>
					</button>
				</view>
			</view>
		</scroll-view>
		
		<!-- 加载提示 -->
		<view class="loading-modal" v-if="isLoading">
			<view class="loading-content">
				<text class="loading-text">{{ loadingText }}</text>
			</view>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			formData: {
				title: '',
				description: '',
				image: '',
				type: ''
			},
			aiDescription: '',
			isLoading: false,
			loadingText: '',
			workTypes: [
				{ value: 'painting', name: 'AI绘画', icon: '🎨' },
				{ value: 'music', name: 'AI音乐', icon: '🎵' },
				{ value: 'writing', name: 'AI写作', icon: '✍️' },
				{ value: 'code', name: 'AI编程', icon: '💻' },
				{ value: 'video', name: 'AI视频', icon: '🎬' },
				{ value: 'other', name: '其他', icon: '🔧' }
			]
		}
	},
	computed: {
		canSubmit() {
			return this.formData.title && this.formData.description && this.formData.image && this.formData.type
		}
	},
	onLoad() {
		// 页面加载
	},
	methods: {
		chooseImage() {
			uni.chooseImage({
				count: 1,
				sizeType: ['compressed'],
				sourceType: ['album', 'camera'],
				success: (res) => {
					this.formData.image = res.tempFilePaths[0]
				}
			})
		},
		selectType(type) {
			this.formData.type = type
		},
		generateAIDescription() {
			if (!this.formData.title || !this.formData.description) {
				uni.showToast({
					title: '请先填写标题和描述',
					icon: 'none'
				})
				return
			}
			
			this.isLoading = true
			this.loadingText = 'AI正在分析您的作品...'
			
			// 模拟AI生成简介
			setTimeout(() => {
				this.aiDescription = `基于您的作品"${this.formData.title}"，AI为您生成了以下简介：

这是一件充满创意的${this.getTypeName(this.formData.type)}作品，展现了作者在AI技术应用方面的独特见解。作品融合了技术与艺术，体现了人工智能在创作领域的无限可能。

推荐标签：#${this.getTypeName(this.formData.type)} #AI创作 #技术创新 #数字艺术`
				
				this.isLoading = false
			}, 2000)
		},
		getTypeName(type) {
			const typeMap = {
				'painting': 'AI绘画',
				'music': 'AI音乐',
				'writing': 'AI写作',
				'code': 'AI编程',
				'video': 'AI视频',
				'other': '创意作品'
			}
			return typeMap[type] || '作品'
		},
		submitWork() {
			if (!this.canSubmit) {
				uni.showToast({
					title: '请完善所有必填信息',
					icon: 'none'
				})
				return
			}
			
			this.isLoading = true
			this.loadingText = '正在发布作品...'
			
			// 模拟提交作品
			setTimeout(() => {
				this.isLoading = false
				uni.showToast({
					title: '作品发布成功！',
					icon: 'success'
				})
				
				// 返回展馆页面
				setTimeout(() => {
					uni.navigateBack()
				}, 1500)
			}, 2000)
		},
		goBack() {
			uni.navigateBack()
		}
	}
}
</script>

<style scoped>
.upload-container {
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

.upload-form {
	background: #ffffff;
	border-radius: 24rpx;
	padding: 32rpx;
	box-shadow: 0 8rpx 32rpx rgba(0, 0, 0, 0.08);
	border: 1px solid rgba(0, 0, 0, 0.05);
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

.form-textarea {
	width: 100%;
	min-height: 160rpx;
	padding: 24rpx;
	background: #f8f9fa;
	border-radius: 16rpx;
	border: 2rpx solid #ecf0f1;
	font-size: 28rpx;
	color: #2c3e50;
	line-height: 1.6;
	transition: border-color 0.2s ease;
}

.form-textarea:focus {
	border-color: #3498db;
}

.upload-area {
	width: 100%;
	height: 200rpx;
	border: 2rpx dashed #bdc3c7;
	border-radius: 16rpx;
	display: flex;
	align-items: center;
	justify-content: center;
	background: #f8f9fa;
	transition: all 0.2s ease;
}

.upload-area:active {
	border-color: #3498db;
	background: #ecf0f1;
}

.upload-placeholder {
	display: flex;
	flex-direction: column;
	align-items: center;
}

.upload-icon {
	font-size: 48rpx;
	margin-bottom: 12rpx;
}

.upload-text {
	font-size: 24rpx;
	color: #7f8c8d;
}

.upload-preview {
	width: 100%;
	height: 100%;
	border-radius: 14rpx;
}

.type-selector {
	display: grid;
	grid-template-columns: 1fr 1fr 1fr;
	gap: 16rpx;
}

.type-option {
	display: flex;
	flex-direction: column;
	align-items: center;
	padding: 24rpx 16rpx;
	background: #f8f9fa;
	border-radius: 16rpx;
	border: 2rpx solid #ecf0f1;
	transition: all 0.2s ease;
}

.type-option-active {
	background: #3498db;
	border-color: #2980b9;
}

.type-option-active .type-name {
	color: #ffffff;
}

.type-icon {
	font-size: 32rpx;
	margin-bottom: 8rpx;
}

.type-name {
	font-size: 22rpx;
	color: #2c3e50;
	font-weight: 600;
}

.ai-generate-btn {
	width: 100%;
	height: 72rpx;
	background: linear-gradient(135deg, #9b59b6, #8e44ad);
	color: #ffffff;
	border: none;
	border-radius: 36rpx;
	font-size: 28rpx;
	font-weight: 600;
	display: flex;
	align-items: center;
	justify-content: center;
	box-shadow: 0 8rpx 24rpx rgba(155, 89, 182, 0.3);
	transition: all 0.2s ease;
}

.ai-generate-btn:active {
	transform: scale(0.98);
}

.ai-description {
	background: #f8f9fa;
	padding: 24rpx;
	border-radius: 16rpx;
	margin-top: 16rpx;
	border-left: 4rpx solid #9b59b6;
}

.ai-desc-label {
	display: block;
	font-size: 24rpx;
	color: #7f8c8d;
	margin-bottom: 8rpx;
}

.ai-desc-text {
	font-size: 26rpx;
	color: #2c3e50;
	line-height: 1.6;
	white-space: pre-line;
}

.submit-section {
	margin-top: 40rpx;
}

.submit-btn {
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

.submit-btn:disabled {
	background: #bdc3c7;
	box-shadow: none;
}

.submit-btn:active:not(:disabled) {
	transform: scale(0.98);
}

.btn-text {
	font-size: 32rpx;
	font-weight: 600;
}

.loading-modal {
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

.loading-content {
	background: #ffffff;
	border-radius: 24rpx;
	padding: 40rpx;
	text-align: center;
	box-shadow: 0 16rpx 48rpx rgba(0, 0, 0, 0.2);
}

.loading-text {
	font-size: 28rpx;
	color: #2c3e50;
}
</style>
