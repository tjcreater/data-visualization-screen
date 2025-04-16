<template>
  <div class="sankey-vertical-container">
    <div class="title">盐湖产品行业流向</div>
    <div class="chart-container" ref="chartRef"></div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue';
import * as echarts from 'echarts';

const chartRef = ref<HTMLElement | null>(null);
let chart: echarts.ECharts | null = null;

// 盐湖行业模拟数据
const sankeyData = [
  { name: '原盐' },
  { name: '工业盐' },
  { name: '食用盐' },
  { name: '化工原料' },
  { name: '氯碱工业' },
  { name: '纯碱生产' },
  { name: '食品加工' },
  { name: '餐饮业' },
  { name: '零售业' },
  { name: '化肥生产' },
  { name: '塑料制品' },
  { name: '玻璃制造' }
];

const sankeyLinks = [
  { source: '原盐', target: '工业盐', value: 5 },
  { source: '原盐', target: '食用盐', value: 3 },
  { source: '工业盐', target: '化工原料', value: 3 },
  { source: '工业盐', target: '氯碱工业', value: 2 },
  { source: '化工原料', target: '纯碱生产', value: 1.5 },
  { source: '化工原料', target: '化肥生产', value: 1.5 },
  { source: '氯碱工业', target: '塑料制品', value: 2 },
  { source: '纯碱生产', target: '玻璃制造', value: 1.5 },
  { source: '食用盐', target: '食品加工', value: 2 },
  { source: '食用盐', target: '餐饮业', value: 0.5 },
  { source: '食用盐', target: '零售业', value: 0.5 }
];

// 初始化图表
const initChart = () => {
  if (!chartRef.value) return;
  
  chart = echarts.init(chartRef.value);
  
  // 设置垂直方向的桑基图配置
  const option = {
    backgroundColor: 'transparent',
    title: {
      show: false
    },
    tooltip: {
      trigger: 'item',
      triggerOn: 'mousemove',
      formatter: '{b}: {c}'
    },
    series: [
      {
        type: 'sankey',
        orient: 'vertical', // 设置为垂直方向
        emphasis: {
          focus: 'adjacency'
        },
        data: sankeyData,
        links: sankeyLinks,
        lineStyle: {
          color: 'gradient',
          curveness: 0.5
        },
        itemStyle: {
          color: '#1f78b4',
          borderColor: '#1f78b4'
        },
        label: {
          color: '#fff',
          fontFamily: 'Arial',
          fontSize: 20
        }
      }
    ]
  };
  
  chart.setOption(option);
};

// 窗口大小变化时重新调整图表大小
const handleResize = () => {
  chart?.resize();
};

onMounted(() => {
  initChart();
  window.addEventListener('resize', handleResize);
});

onUnmounted(() => {
  chart?.dispose();
  window.removeEventListener('resize', handleResize);
});
</script>

<style scoped>
.sankey-vertical-container {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  background-color: rgba(0, 0, 0, 0.2);
  padding: 15px;
  box-sizing: border-box;
  border-radius: 4px;
  
  .title {
    font-size: 32px;
    font-weight: bold;
    color: #fff;
    margin-bottom: 15px;
    text-align: center;
  }
  
  .chart-container {
    flex: 1;
    width: 100%;
    height: 100%;
    min-height: 300px;
  }
}
</style>