<script>
	export default {
		onLaunch: function() {
			console.log('AI血缘 App Launch')
			// 初始化应用
			this.initApp()
		},
		onShow: function() {
			console.log('AI血缘 App Show')
		},
		onHide: function() {
			console.log('AI血缘 App Hide')
		},
		methods: {
			initApp() {
				// 设置状态栏样式
				uni.setNavigationBarColor({
					frontColor: '#000000',
					backgroundColor: '#ffffff'
				})
				
				// 初始化用户数据
				this.initUserData()
			},
			initUserData() {
				// 检查是否首次启动
				uni.getStorage({
					key: 'isFirstLaunch',
					success: (res) => {
						// 非首次启动
					},
					fail: () => {
						// 首次启动，设置默认数据
						this.setDefaultData()
					}
				})
			},
			setDefaultData() {
				// 设置默认用户信息
				const defaultUser = {
					name: 'AI学习者',
					title: '机器学习工程师',
					description: '热爱AI技术，专注于深度学习与自然语言处理',
					avatar: '/static/avatar.jpg'
				}
				
				// 设置默认设置
				const defaultSettings = {
					themeMode: 'light',
					notifications: {
						learning: true,
						likes: true,
						updates: false
					}
				}
				
				uni.setStorage({
					key: 'userInfo',
					data: defaultUser
				})
				
				uni.setStorage({
					key: 'userSettings',
					data: defaultSettings
				})
				
				uni.setStorage({
					key: 'isFirstLaunch',
					data: false
				})
			}
		}
	}
</script>

<style>
	/* 全局样式 - 水彩画风格（青绿到蓝色渐变） */
	page {
		background: linear-gradient(135deg, #00CED1 0%, #40E0D0 20%, #00CED1 40%, #1E90FF 60%, #0000CD 80%, #00CED1 100%);
		font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'PingFang SC', 'Hiragino Sans GB', 'Microsoft YaHei', sans-serif;
		line-height: 1.6;
		color: #333333;
		position: relative;
		overflow-x: hidden;
	}
	
	/* 水彩画效果背景 */
	page::before {
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
	
	/* 通用样式 - 二次元风格 */
	.container {
		padding: 32rpx;
	}
	
	.card {
		background: rgba(255, 255, 255, 0.95);
		border-radius: 8rpx;
		box-shadow: 0 4rpx 20rpx rgba(0, 206, 209, 0.2);
		border: 2rpx solid rgba(0, 206, 209, 0.3);
		position: relative;
		overflow: hidden;
		backdrop-filter: blur(10rpx);
	}
	
	.btn {
		height: 88rpx;
		border-radius: 8rpx;
		font-size: 32rpx;
		font-weight: 700;
		display: flex;
		align-items: center;
		justify-content: center;
		transition: all 0.2s ease;
		border: none;
		position: relative;
		overflow: hidden;
	}
	
	.btn-primary {
		background: linear-gradient(135deg, #00CED1, #1E90FF);
		color: #ffffff;
		box-shadow: 0 6rpx 20rpx rgba(0, 206, 209, 0.4);
		border: 2rpx solid rgba(0, 206, 209, 0.5);
	}
	
	.btn-secondary {
		background: rgba(255, 255, 255, 0.95);
		color: #00CED1;
		border: 2rpx solid #00CED1;
		box-shadow: 0 6rpx 20rpx rgba(0, 206, 209, 0.2);
		backdrop-filter: blur(10rpx);
	}
	
	.btn:active {
		transform: scale(0.98);
	}
	
	/* 文本样式 - 水彩画风格 */
	.title {
		font-size: 36rpx;
		font-weight: 700;
		color: #00CED1;
	}
	
	.subtitle {
		font-size: 28rpx;
		color: #535c68;
	}
	
	.text-primary {
		color: #00CED1;
	}
	
	.text-secondary {
		color: #535c68;
	}
	
	.text-success {
		color: #1E90FF;
	}
	
	.text-danger {
		color: #ff4757;
	}
	
	.text-warning {
		color: #ffa502;
	}
	
	/* 布局样式 */
	.flex {
		display: flex;
	}
	
	.flex-column {
		flex-direction: column;
	}
	
	.flex-center {
		align-items: center;
		justify-content: center;
	}
	
	.flex-between {
		justify-content: space-between;
	}
	
	.flex-1 {
		flex: 1;
	}
	
	/* 间距样式 */
	.mt-16 { margin-top: 16rpx; }
	.mt-24 { margin-top: 24rpx; }
	.mt-32 { margin-top: 32rpx; }
	.mt-40 { margin-top: 40rpx; }
	
	.mb-16 { margin-bottom: 16rpx; }
	.mb-24 { margin-bottom: 24rpx; }
	.mb-32 { margin-bottom: 32rpx; }
	.mb-40 { margin-bottom: 40rpx; }
	
	.p-16 { padding: 16rpx; }
	.p-24 { padding: 24rpx; }
	.p-32 { padding: 32rpx; }
	.p-40 { padding: 40rpx; }
	
	/* 动画效果 */
	.fade-in {
		animation: fadeIn 0.3s ease-in-out;
	}
	
	.slide-up {
		animation: slideUp 0.3s ease-out;
	}
	
	@keyframes fadeIn {
		from { opacity: 0; }
		to { opacity: 1; }
	}
	
	@keyframes slideUp {
		from { 
			opacity: 0;
			transform: translateY(30rpx);
		}
		to { 
			opacity: 1;
			transform: translateY(0);
		}
	}
	
	/* 响应式设计 */
	@media screen and (max-width: 750rpx) {
		.container {
			padding: 24rpx;
		}
		
		.title {
			font-size: 32rpx;
		}
		
		.btn {
			height: 80rpx;
			font-size: 28rpx;
		}
	}
</style>
