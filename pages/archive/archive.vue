<template>
	<view class="archive-container">
		<!-- 自定义导航栏 -->
		<view class="custom-navbar">
			<view class="navbar-content">
				<text class="navbar-title">学习档案</text>
			</view>
		</view>
		
		<!-- 主体内容 -->
		<scroll-view class="content" scroll-y>
			<!-- 学习统计 -->
			<view class="stats-section">
				<view class="stats-card">
					<view class="stat-item">
						<text class="stat-number">{{ totalDays }}</text>
						<text class="stat-label">学习天数</text>
					</view>
					<view class="stat-item">
						<text class="stat-number">{{ totalHours }}</text>
						<text class="stat-label">学习时长</text>
					</view>
					<view class="stat-item">
						<text class="stat-number">{{ completedProjects }}</text>
						<text class="stat-label">完成项目</text>
					</view>
				</view>
			</view>
			
			<!-- AI学习时间线 -->
			<view class="timeline-section">
				<view class="section-header">
					<text class="section-title">AI学习时间线</text>
				</view>
				<view class="timeline">
					<view 
						class="timeline-item" 
						v-for="(item, index) in timelineData" 
						:key="index"
						:class="{ 'timeline-item-active': index === 0 }"
					>
						<view class="timeline-dot"></view>
						<view class="timeline-content">
							<view class="timeline-date">{{ item.date }}</view>
							<view class="timeline-title">{{ item.title }}</view>
							<view class="timeline-desc">{{ item.description }}</view>
							<view class="timeline-tags">
								<text 
									class="timeline-tag" 
									v-for="tag in item.tags" 
									:key="tag"
								>{{ tag }}</text>
							</view>
						</view>
					</view>
				</view>
			</view>
			
			<!-- 操作按钮 -->
			<view class="action-buttons">
				<button class="action-btn primary" @click="goToLineage">
					<text class="btn-text">查看血缘图谱</text>
				</button>
				<button class="action-btn secondary" @click="generateReport">
					<text class="btn-text">生成学习报告</text>
				</button>
			</view>
		</scroll-view>
		
		<!-- 自定义底部导航栏 -->
		<custom-tabbar :current="1" @change="onTabChange"></custom-tabbar>
		
		<!-- AI报告弹窗 -->
		<view class="report-modal" v-if="showReport" @click="closeReport">
			<view class="report-content" @click.stop>
				<view class="report-header">
					<text class="report-title">AI学习报告</text>
					<text class="report-close" @click="closeReport">×</text>
				</view>
				<view class="report-body">
					<text class="report-text">{{ aiReport }}</text>
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
			totalDays: 45,
			totalHours: 128,
			completedProjects: 12,
			showReport: false,
			aiReport: '',
			timelineData: [
				{
					date: '2024-01-15',
					title: '完成深度学习基础课程',
					description: '掌握了神经网络的基本原理和实现方法，完成了手写数字识别项目。',
					tags: ['深度学习', 'Python', 'TensorFlow']
				},
				{
					date: '2024-01-10',
					title: '学习机器学习算法',
					description: '深入理解了监督学习、无监督学习和强化学习的核心概念。',
					tags: ['机器学习', '算法', '数学']
				},
				{
					date: '2024-01-05',
					title: '开始AI学习之旅',
					description: '正式踏入人工智能领域，从基础概念开始学习。',
					tags: ['入门', '基础', '概念']
				},
				{
					date: '2024-01-01',
					title: '制定学习计划',
					description: '为自己制定了详细的AI学习路线图，设定了阶段性目标。',
					tags: ['规划', '目标', '计划']
				}
			]
		}
	},
	onLoad() {
		this.loadTimelineData()
	},
	methods: {
		loadTimelineData() {
			// 模拟加载时间线数据
			// 这里可以从API获取真实数据
		},
		goToLineage() {
			uni.navigateTo({
				url: '/pages/lineage/lineage'
			})
		},
		generateReport() {
			// 模拟AI生成报告
			this.aiReport = `基于您的学习数据，AI为您生成了这份学习报告：

📊 学习概况
• 总学习时长：${this.totalHours}小时
• 学习天数：${this.totalDays}天
• 完成项目：${this.completedProjects}个

🎯 学习亮点
• 深度学习基础扎实，已掌握核心算法
• 编程实践能力强，项目完成度高
• 学习持续性良好，保持每日学习习惯

🚀 建议方向
• 可以尝试更高级的模型架构
• 建议参与开源项目，提升实战经验
• 考虑学习最新的AI技术趋势

继续加油，AI学习之路越走越精彩！✨`
			
			this.showReport = true
		},
		closeReport() {
			this.showReport = false
		},
		onTabChange(index) {
			// 处理底部导航栏切换
			console.log('Tab changed to:', index)
		}
	}
}
</script>

<style scoped>
.archive-container {
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

.stats-section {
	margin-bottom: 40rpx;
}

.stats-card {
	background: #ffffff;
	border-radius: 24rpx;
	padding: 32rpx;
	box-shadow: 0 8rpx 32rpx rgba(0, 0, 0, 0.08);
	border: 1px solid rgba(0, 0, 0, 0.05);
	display: flex;
	justify-content: space-around;
}

.stat-item {
	display: flex;
	flex-direction: column;
	align-items: center;
}

.stat-number {
	font-size: 48rpx;
	font-weight: 700;
	color: #3498db;
	margin-bottom: 8rpx;
}

.stat-label {
	font-size: 24rpx;
	color: #7f8c8d;
}

.timeline-section {
	margin-bottom: 40rpx;
}

.section-header {
	margin-bottom: 32rpx;
}

.section-title {
	font-size: 32rpx;
	font-weight: 600;
	color: #2c3e50;
}

.timeline {
	position: relative;
	padding-left: 40rpx;
}

.timeline::before {
	content: '';
	position: absolute;
	left: 20rpx;
	top: 0;
	bottom: 0;
	width: 2rpx;
	background: linear-gradient(180deg, #3498db, #e9ecef);
}

.timeline-item {
	position: relative;
	margin-bottom: 48rpx;
	display: flex;
	align-items: flex-start;
}

.timeline-item:last-child {
	margin-bottom: 0;
}

.timeline-dot {
	position: absolute;
	left: -40rpx;
	top: 16rpx;
	width: 20rpx;
	height: 20rpx;
	background: #3498db;
	border-radius: 50%;
	border: 4rpx solid #ffffff;
	box-shadow: 0 0 0 4rpx rgba(52, 152, 219, 0.2);
}

.timeline-item-active .timeline-dot {
	background: #e74c3c;
	box-shadow: 0 0 0 4rpx rgba(231, 76, 60, 0.2);
	animation: pulse 2s infinite;
}

@keyframes pulse {
	0% { transform: scale(1); }
	50% { transform: scale(1.1); }
	100% { transform: scale(1); }
}

.timeline-content {
	background: #ffffff;
	border-radius: 20rpx;
	padding: 32rpx;
	box-shadow: 0 8rpx 32rpx rgba(0, 0, 0, 0.08);
	border: 1px solid rgba(0, 0, 0, 0.05);
	flex: 1;
}

.timeline-date {
	font-size: 24rpx;
	color: #7f8c8d;
	margin-bottom: 12rpx;
}

.timeline-title {
	font-size: 32rpx;
	font-weight: 600;
	color: #2c3e50;
	margin-bottom: 16rpx;
}

.timeline-desc {
	font-size: 28rpx;
	color: #5a6c7d;
	line-height: 1.6;
	margin-bottom: 20rpx;
}

.timeline-tags {
	display: flex;
	flex-wrap: wrap;
	gap: 12rpx;
}

.timeline-tag {
	background: #ecf0f1;
	color: #3498db;
	font-size: 22rpx;
	padding: 8rpx 16rpx;
	border-radius: 20rpx;
}

.action-buttons {
	display: flex;
	flex-direction: column;
	gap: 24rpx;
}

.action-btn {
	height: 88rpx;
	border-radius: 44rpx;
	border: none;
	font-size: 32rpx;
	font-weight: 600;
	display: flex;
	align-items: center;
	justify-content: center;
	transition: all 0.2s ease;
}

.action-btn.primary {
	background: linear-gradient(135deg, #3498db, #2980b9);
	color: #ffffff;
	box-shadow: 0 8rpx 24rpx rgba(52, 152, 219, 0.3);
}

.action-btn.secondary {
	background: #ffffff;
	color: #3498db;
	border: 2rpx solid #3498db;
	box-shadow: 0 8rpx 24rpx rgba(0, 0, 0, 0.08);
}

.action-btn:active {
	transform: scale(0.98);
}

.btn-text {
	font-size: 32rpx;
	font-weight: 600;
}

.report-modal {
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

.report-content {
	background: #ffffff;
	border-radius: 24rpx;
	margin: 32rpx;
	max-height: 80vh;
	overflow: hidden;
	box-shadow: 0 16rpx 48rpx rgba(0, 0, 0, 0.2);
}

.report-header {
	display: flex;
	justify-content: space-between;
	align-items: center;
	padding: 32rpx;
	border-bottom: 1px solid #ecf0f1;
}

.report-title {
	font-size: 36rpx;
	font-weight: 600;
	color: #2c3e50;
}

.report-close {
	font-size: 48rpx;
	color: #7f8c8d;
	line-height: 1;
}

.report-body {
	padding: 32rpx;
	max-height: 60vh;
	overflow-y: auto;
}

.report-text {
	font-size: 28rpx;
	color: #2c3e50;
	line-height: 1.8;
	white-space: pre-line;
}
</style>
