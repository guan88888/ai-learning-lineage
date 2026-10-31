<template>
	<view class="lineage-container">
		<!-- 自定义导航栏 -->
		<view class="custom-navbar">
			<view class="navbar-content">
				<text class="navbar-title">AI血缘图谱</text>
				<text class="navbar-back" @click="goBack">← 返回</text>
			</view>
		</view>
		
		<!-- 主体内容 -->
		<scroll-view class="content" scroll-y>
			<!-- 图谱说明 -->
			<view class="intro-section">
				<view class="intro-card">
					<text class="intro-title">AI血缘关系网络</text>
					<text class="intro-desc">展示您的学习路径、项目关联和知识传承关系</text>
				</view>
			</view>
			
			<!-- 网络节点图 -->
			<view class="network-section">
				<view class="network-container">
					<!-- 中心节点 -->
					<view class="center-node" @click="showNodeDetail('center')">
						<view class="node-avatar">
							<text class="node-icon">🧠</text>
						</view>
						<text class="node-label">AI学习者</text>
					</view>
					
					<!-- 学习节点 -->
					<view 
						class="learning-node" 
						v-for="(node, index) in learningNodes" 
						:key="node.id"
						:style="getNodePosition(index, 'learning')"
						@click="showNodeDetail(node)"
					>
						<view class="node-avatar learning">
							<text class="node-icon">{{ node.icon }}</text>
						</view>
						<text class="node-label">{{ node.name }}</text>
					</view>
					
					<!-- 项目节点 -->
					<view 
						class="project-node" 
						v-for="(node, index) in projectNodes" 
						:key="node.id"
						:style="getNodePosition(index, 'project')"
						@click="showNodeDetail(node)"
					>
						<view class="node-avatar project">
							<text class="node-icon">{{ node.icon }}</text>
						</view>
						<text class="node-label">{{ node.name }}</text>
					</view>
					
					<!-- 人物节点 -->
					<view 
						class="person-node" 
						v-for="(node, index) in personNodes" 
						:key="node.id"
						:style="getNodePosition(index, 'person')"
						@click="showNodeDetail(node)"
					>
						<view class="node-avatar person">
							<text class="node-icon">{{ node.icon }}</text>
						</view>
						<text class="node-label">{{ node.name }}</text>
					</view>
					
					<!-- 连接线 -->
					<view class="connections">
						<view 
							class="connection-line" 
							v-for="(connection, index) in connections" 
							:key="index"
							:style="getConnectionStyle(connection)"
						></view>
					</view>
				</view>
			</view>
			
			<!-- 节点详情 -->
			<view class="node-detail" v-if="selectedNode">
				<view class="detail-card">
					<view class="detail-header">
						<text class="detail-title">{{ selectedNode.name }}</text>
						<text class="detail-close" @click="closeNodeDetail">×</text>
					</view>
					<view class="detail-body">
						<text class="detail-desc">{{ selectedNode.description }}</text>
						<view class="detail-tags">
							<text 
								class="detail-tag" 
								v-for="tag in selectedNode.tags" 
								:key="tag"
							>{{ tag }}</text>
						</view>
					</view>
				</view>
			</view>
			
			<!-- 操作按钮 -->
			<view class="action-buttons">
				<button class="action-btn secondary" @click="goBack">
					<text class="btn-text">返回学习档案</text>
				</button>
			</view>
		</scroll-view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			selectedNode: null,
			learningNodes: [
				{
					id: 'ml',
					name: '机器学习',
					icon: '🤖',
					description: '掌握监督学习、无监督学习和强化学习的核心算法',
					tags: ['算法', '数学', '统计']
				},
				{
					id: 'dl',
					name: '深度学习',
					icon: '🧠',
					description: '深入理解神经网络架构和优化方法',
					tags: ['神经网络', 'CNN', 'RNN']
				},
				{
					id: 'nlp',
					name: '自然语言处理',
					icon: '💬',
					description: '文本分析和语言模型的应用',
					tags: ['文本', '语言模型', 'NLP']
				}
			],
			projectNodes: [
				{
					id: 'project1',
					name: '图像识别',
					icon: '👁️',
					description: '基于CNN的图像分类项目',
					tags: ['计算机视觉', 'CNN', '分类']
				},
				{
					id: 'project2',
					name: '文本生成',
					icon: '📝',
					description: '使用Transformer的文本生成模型',
					tags: ['NLP', '生成模型', 'Transformer']
				}
			],
			personNodes: [
				{
					id: 'mentor1',
					name: '导师A',
					icon: '👨‍🏫',
					description: '机器学习领域的资深专家',
					tags: ['导师', '专家', '指导']
				},
				{
					id: 'peer1',
					name: '同学B',
					icon: '👨‍💻',
					description: '深度学习研究伙伴',
					tags: ['同学', '研究', '合作']
				}
			],
			connections: [
				{ from: 'center', to: 'ml', type: 'learning' },
				{ from: 'center', to: 'dl', type: 'learning' },
				{ from: 'center', to: 'nlp', type: 'learning' },
				{ from: 'ml', to: 'project1', type: 'project' },
				{ from: 'dl', to: 'project1', type: 'project' },
				{ from: 'nlp', to: 'project2', type: 'project' },
				{ from: 'center', to: 'mentor1', type: 'person' },
				{ from: 'center', to: 'peer1', type: 'person' }
			]
		}
	},
	onLoad() {
		this.initNetwork()
	},
	methods: {
		initNetwork() {
			// 初始化网络布局
		},
		getNodePosition(index, type) {
			const positions = {
				learning: [
					{ top: '20%', left: '10%' },
					{ top: '40%', left: '80%' },
					{ top: '60%', left: '15%' }
				],
				project: [
					{ top: '30%', left: '30%' },
					{ top: '50%', left: '70%' }
				],
				person: [
					{ top: '10%', left: '50%' },
					{ top: '70%', left: '50%' }
				]
			}
			return positions[type][index] || { top: '50%', left: '50%' }
		},
		getConnectionStyle(connection) {
			// 简化的连接线样式
			return {
				position: 'absolute',
				top: '50%',
				left: '50%',
				width: '100rpx',
				height: '2rpx',
				background: '#3498db',
				transform: 'rotate(45deg)',
				opacity: 0.6
			}
		},
		showNodeDetail(node) {
			if (node === 'center') {
				this.selectedNode = {
					name: 'AI学习者',
					description: '您是这个AI血缘网络的核心，连接着各种学习资源和项目',
					tags: ['核心', '学习者', '连接者']
				}
			} else {
				this.selectedNode = node
			}
		},
		closeNodeDetail() {
			this.selectedNode = null
		},
		goBack() {
			uni.navigateBack()
		}
	}
}
</script>

<style scoped>
.lineage-container {
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

.intro-section {
	margin-bottom: 32rpx;
}

.intro-card {
	background: #ffffff;
	border-radius: 24rpx;
	padding: 32rpx;
	box-shadow: 0 8rpx 32rpx rgba(0, 0, 0, 0.08);
	border: 1px solid rgba(0, 0, 0, 0.05);
	text-align: center;
}

.intro-title {
	display: block;
	font-size: 32rpx;
	font-weight: 600;
	color: #2c3e50;
	margin-bottom: 16rpx;
}

.intro-desc {
	font-size: 26rpx;
	color: #7f8c8d;
	line-height: 1.6;
}

.network-section {
	margin-bottom: 32rpx;
}

.network-container {
	position: relative;
	height: 600rpx;
	background: #ffffff;
	border-radius: 24rpx;
	box-shadow: 0 8rpx 32rpx rgba(0, 0, 0, 0.08);
	border: 1px solid rgba(0, 0, 0, 0.05);
	overflow: hidden;
}

.center-node {
	position: absolute;
	top: 50%;
	left: 50%;
	transform: translate(-50%, -50%);
	display: flex;
	flex-direction: column;
	align-items: center;
	z-index: 10;
}

.node-avatar {
	width: 80rpx;
	height: 80rpx;
	border-radius: 40rpx;
	background: linear-gradient(135deg, #3498db, #2980b9);
	display: flex;
	align-items: center;
	justify-content: center;
	margin-bottom: 12rpx;
	box-shadow: 0 8rpx 24rpx rgba(52, 152, 219, 0.3);
	animation: pulse 2s infinite;
}

.node-avatar.learning {
	background: linear-gradient(135deg, #e74c3c, #c0392b);
	box-shadow: 0 8rpx 24rpx rgba(231, 76, 60, 0.3);
}

.node-avatar.project {
	background: linear-gradient(135deg, #f39c12, #e67e22);
	box-shadow: 0 8rpx 24rpx rgba(243, 156, 18, 0.3);
}

.node-avatar.person {
	background: linear-gradient(135deg, #9b59b6, #8e44ad);
	box-shadow: 0 8rpx 24rpx rgba(155, 89, 182, 0.3);
}

@keyframes pulse {
	0% { transform: scale(1); }
	50% { transform: scale(1.05); }
	100% { transform: scale(1); }
}

.node-icon {
	font-size: 32rpx;
	color: #ffffff;
}

.node-label {
	font-size: 20rpx;
	color: #2c3e50;
	text-align: center;
	font-weight: 600;
}

.learning-node,
.project-node,
.person-node {
	position: absolute;
	display: flex;
	flex-direction: column;
	align-items: center;
	z-index: 5;
	transition: transform 0.2s ease;
}

.learning-node:active,
.project-node:active,
.person-node:active {
	transform: scale(1.1);
}

.connections {
	position: absolute;
	top: 0;
	left: 0;
	right: 0;
	bottom: 0;
	z-index: 1;
}

.connection-line {
	position: absolute;
	background: #3498db;
	opacity: 0.6;
}

.node-detail {
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

.detail-card {
	background: #ffffff;
	border-radius: 24rpx;
	margin: 32rpx;
	max-width: 600rpx;
	box-shadow: 0 16rpx 48rpx rgba(0, 0, 0, 0.2);
}

.detail-header {
	display: flex;
	justify-content: space-between;
	align-items: center;
	padding: 32rpx;
	border-bottom: 1px solid #ecf0f1;
}

.detail-title {
	font-size: 32rpx;
	font-weight: 600;
	color: #2c3e50;
}

.detail-close {
	font-size: 48rpx;
	color: #7f8c8d;
	line-height: 1;
}

.detail-body {
	padding: 32rpx;
}

.detail-desc {
	font-size: 28rpx;
	color: #2c3e50;
	line-height: 1.6;
	margin-bottom: 24rpx;
}

.detail-tags {
	display: flex;
	flex-wrap: wrap;
	gap: 12rpx;
}

.detail-tag {
	background: #ecf0f1;
	color: #3498db;
	font-size: 22rpx;
	padding: 8rpx 16rpx;
	border-radius: 20rpx;
}

.action-buttons {
	display: flex;
	justify-content: center;
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

.action-btn.secondary {
	background: #ffffff;
	color: #3498db;
	border: 2rpx solid #3498db;
	box-shadow: 0 8rpx 24rpx rgba(0, 0, 0, 0.08);
	width: 100%;
}

.action-btn:active {
	transform: scale(0.98);
}

.btn-text {
	font-size: 32rpx;
	font-weight: 600;
}
</style>
