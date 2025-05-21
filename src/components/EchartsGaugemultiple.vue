<template> 
    <div class="gauge-wrapper">
      <div class="gauge-header">
        <h3 class="gauge-title">{{ title }}</h3>
      </div>
      <div ref="chartContainer" class="chart-box"></div>
    </div>
  </template>
  
  <script setup lang="ts">
  import { ref, onMounted, watch, onUnmounted, computed } from 'vue';
  import * as echarts from 'echarts/core';
  import { GaugeChart } from 'echarts/charts';
  import { CanvasRenderer } from 'echarts/renderers';
  import { TitleComponent, TooltipComponent, LegendComponent } from 'echarts/components';
  
  echarts.use([
    TitleComponent,
    TooltipComponent,
    LegendComponent,
    GaugeChart,
    CanvasRenderer
  ]);
  
  const props = defineProps({
    segments: {
      type: Array as () => Array<{
        value: number,
        name: string,
        color: string,
        min: number,
        max: number
      }>,
      required: true
    },
    title: {
      type: String,
      default: 'Comparación de Regiones'
    }
  });
  
  const chartContainer = ref<HTMLElement | null>(null);
  let chart: echarts.ECharts | null = null;
  
  const chartOptions = computed(() => {
    const count = props.segments.length;
    const radius = count > 1 ? '55%' : '70%';
    const centerStep = 100 / (count + 1);
  
    const series = props.segments.map((segment, index) => {
      const centerX = (index + 1) * centerStep;
      return {
        type: 'gauge',
        radius,
        center: [`${centerX}%`, '60%'],
        startAngle: 90,
        endAngle: -270,
        min: segment.min,
        max: segment.max,
        itemStyle: {
          color: segment.color
        },
        progress: {
          show: true,
          width: 6,
          roundCap: true
        },
        pointer: {
          show: false
        },
        axisLine: {
          lineStyle: {
            width: 10,
            color: [
              [1, '#e0e0e0']
            ]
          }
        },
        axisTick: { show: false },
        splitLine: { show: false },
        axisLabel: {
          distance: -55,
          fontSize: 11,
          color: '#999',
          formatter: (value: number) =>
            value === segment.min || value === segment.max ? `${value}%` : ''
        },
        title: {
          show: true,
          offsetCenter: [0, '120%'],
          fontSize: 12,
          color: '#555',
          fontWeight: 500
        },
        detail: {
          valueAnimation: true,
          fontSize: 16,
          fontWeight: 'bold',
          offsetCenter: [0, '60%'],
          color: segment.color,
          formatter: '{value}%'
        },
        data: [{
          value: segment.value,
          name: segment.name
        }]
      };
    });
  
    return {
      backgroundColor: 'transparent',
      tooltip: {
        formatter: '{a} <br/>{c}% ({b})'
      },
      series
    };
  });
  
  const initChart = () => {
    if (chartContainer.value) {
      chart = echarts.init(chartContainer.value);
      updateChart();
  
      const resizeObserver = new ResizeObserver(() => {
        if (chart) chart.resize();
      });
  
      resizeObserver.observe(chartContainer.value);
  
      onUnmounted(() => {
        resizeObserver.disconnect();
        if (chart) {
          chart.dispose();
          chart = null;
        }
      });
    }
  };
  
  const updateChart = () => {
    if (chart) {
      chart.setOption(chartOptions.value, true);
    }
  };
  
  watch(() => props.segments, () => {
    updateChart();
  }, { deep: true });
  
  onMounted(() => {
    initChart();
  });
  </script>
  
  <style scoped>
  .gauge-wrapper {
    display: flex;
    flex-direction: column;
    width: 100%;
    height: 100%;
    padding: 12px;
    box-sizing: border-box;
  }
  
  .gauge-header {
    display: flex;
    justify-content: center;
    align-items: center;
    margin-bottom: 8px;
  }
  
  .gauge-title {
    font-size: 1rem;
    font-weight: 600;
    color: #333;
    margin: 0;
    text-align: center;
  }
  
  .chart-box {
    flex: 1;
    width: 100%;
    height: 100%;
    min-height: 200px;
    max-height: 320px;
  }
  </style>
  