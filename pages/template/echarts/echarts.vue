<template>
	<view class="container">
		<view class="page-section-title" style="display: block;">
			<view class="uni-hello-text uni-common-pb" style="display: block;">
				集成mpvue-echarts的图表示例，用法参考：<u-link :href="'https://github.com/F-loat/mpvue-echarts'" :text="'https://github.com/F-loat/mpvue-echarts'"></u-link>
			</view>
		</view>
		<view class="canvasView">
			<view class="canvas-bar">
				<view class="title">饼图示例</view>
				<button class="update-btn" type="primary" size="mini" @click="updatePie">修改饼状图数据</button>
			</view>
			<mpvue-echarts class="ec-canvas" @onInit="pieInit" canvasId="pie" ref="pieChart" />
		</view>
		<view class="canvasView">
			<view class="title">折线图示例</view>
			<mpvue-echarts class="ec-canvas" @onInit="lineInit" canvasId="line" ref="lineChart" />
		</view>
	</view>
</template>

<script>
	import * as echarts from '@/components/echarts/echarts.simple.min.js';
	import mpvueEcharts from '@/components/mpvue-echarts/src/echarts.vue';
	import uLink from "@/components/uLink.vue"

	const cityList = [{
		value: 55,
		name: '北京'
	}, {
		value: 38,
		name: '上海'
	}, {
		value: 20,
		name: '广州'
	}];

	let pieOption = {
		animation: false,
		backgroundColor: '#F8F8F8',
		color: ['#37A2DA', '#32C5E9', '#67E0E3', '#91F2DE', '#FFDB5C', '#FF9F7F'],
		series: [{
			label: {
				normal: {
					fontSize: 14
				}
			},
			type: 'pie',
			center: ['50%', '50%'],
			radius: [0, '60%'],
			data: [],
			itemStyle: {
				emphasis: {
					shadowBlur: 10,
					shadowOffsetX: 0,
					shadowColor: 'rgba(0, 2, 2, 0.3)'
				}
			}
		}]
	};

	let lineOption = {
		animation: false,
		color: ['#37A2DA', '#9FE6B8'],
		grid: {
			x: 35,
			x2: 10,
			y: 30,
			y2: 25
		},
		calculable: false,
		xAxis: [{
			type: 'category',
			data: ['1月', '2月', '3月', '4月', '5月', '6月', '7月', '8月', '9月', '10月', '11月', '12月']
		}],
		yAxis: [{
			type: 'value',
			splitArea: {
				show: true
			}
		}],
		series: [{
			name: '蒸发量',
			type: 'line',
			data: [2.0, 4.9, 7.0, 23.2, 25.6, 76.7, 135.6, 162.2, 32.6, 20.0, 6.4, 3.3]
		}, {
			name: '降水量',
			type: 'line',
			data: [2.6, 5.9, 9.0, 26.4, 28.7, 70.7, 175.6, 182.2, 48.7, 18.8, 6.0, 2.3]
		}]
	};

	export default {
		data() {
			return {
				// echarts: echarts,
				updateStatus: false
			}
		},
		onLoad() {
			pieOption.series[0].data = cityList.slice(0);
		},
		methods: {
			updatePie() {
				// 参考 mpvue-charts 的懒加载示例
				// https://github.com/F-loat/mpvue-echarts/blob/master/examples/lazyLoad.vue

				if (this.updateStatus) {
					return;
				}
				pieOption.series[0].data.push({
					value: 20,
					name: '武汉'
				});
				pieOption.series[0].data.push({
					value: 10,
					name: '杭州'
				});
				this.$refs.pieChart.init();
				this.updateStatus = true;
			},
			// pieInit(canvas, width, height) {
			pieInit(e) {
				let {canvas, width, height,fire} = e ;
				echarts.setCanvasCreator(() => canvas);
				let pieChart = echarts.init(canvas, null, {
					width: width,
					height: height
				})
				canvas.setChart(pieChart)

				pieChart.setOption(pieOption)
				fire(pieChart)
				// return pieChart
			},
			// lineInit(canvas, width, height) {
			lineInit(e) {
				let {canvas, width, height,fire} = e ;
				echarts.setCanvasCreator(() => canvas);
				let lineChart = echarts.init(canvas, null, {
					width: width,
					height: height
				})
				canvas.setChart(lineChart)

				lineChart.setOption(lineOption)
				fire(lineChart)
				// return lineChart
			}
		},
		components: {
			mpvueEcharts,
			uLink
		}
	}
</script>

<style>
	page,
	view {
		display: flex;
		/* uni-app默认使用flex布局。因为flex布局有利于跨更多平台，尤其是采用原生渲染的平台。如不了解flex布局，请参考http://www.w3.org/TR/css3-flexbox/。若不需要flex布局可删除本行*/
	}

	page {
		min-height: 100%;
	}

	.page-section-title {
		padding: 0 30upx;
	}

	.title {
		margin-left: 30upx;
		color: #8f8f94;
	}

	.container {
		padding-bottom: 30upx;
		box-sizing: border-box;
	}

	.container,
	.canvasView {
		flex: 1;
		flex-direction: column;
	}

	.ec-canvas {
		width: 100%;
		height: 100%;
		flex: 1;
	}

	.canvas-bar {
		display: flex;
		flex-direction: row;
		justify-content: space-between;
	}

	.update-btn {
		margin-right: 30upx;
	}
</style>
