<template>
  <div class="gauge-modern-wrapper">
    <VEChart class="gauge-chart" :option="options" autoresize />
    <div class="gauge-title">
      <span>{{ props.title ?? 'KPI' }}</span>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, watchEffect } from "vue";

// ECharts
import { use } from "echarts/core";
import VEChart from "vue-echarts";
import { GaugeChart } from "echarts/charts";
import { CanvasRenderer } from "echarts/renderers";
import { TooltipComponent } from "echarts/components";

// Register ECharts modules
use([GaugeChart, CanvasRenderer, TooltipComponent]);

const props = defineProps<{ value: number; title?: string }>();

const options = ref({});

watchEffect(() => {
  options.value = {
    backgroundColor: 'transparent',
    series: [
      {
        type: 'gauge',
        center: ['50%', '55%'],
        radius: '95%',
        min: 0,
        max: 100,
        axisLine: {
          lineStyle: {
            width: 22,
            color: [
              [0.2, "#ef4444"],
              [0.69, "#f59e42"],
              [1, "#22d3ee"]
            ],
            shadowColor: '#23213a',
            shadowBlur: 12
          }
        },
        pointer: {
          itemStyle: {
            color: '#fff',
            shadowColor: '#2563eb',
            shadowBlur: 6
          },
          length: '60%'
        },
        axisTick: {
          distance: -24,
          length: 5,
          lineStyle: {
            color: '#e0e7ef',
            width: 1
          }
        },
        splitLine: {
          distance: -24,
          length: 22,
          lineStyle: {
            color: '#fff',
            width: 2
          }
        },
        axisLabel: {
          color: '#e0e7ef',
          distance: 30,
          fontSize: 15,
          fontWeight: 'bold',
          fontFamily: 'Segoe UI, Arial, sans-serif',
          formatter(value: number) {
            return value % 20 === 0 ? value.toString() : '';
          }
        },
        detail: {
          valueAnimation: true,
          formatter: '{value}',
          color: '#fff',
          fontSize: 32,
          fontWeight: 'bold',
          offsetCenter: [0, '20%'],
          fontFamily: 'Segoe UI, Arial, sans-serif',
          shadowColor: '#23213a',
          shadowBlur: 8
        },
        title: {
          show: false
        },
        data: [
          {
            value: props.value,
            name: props.title ?? 'KPI'
          }
        ]
      }
    ]
  };
});
</script>

<style scoped>
.gauge-modern-wrapper {
  width: 100%;
  height: 100%;
  min-height: 300px;
  background: linear-gradient(135deg, #23213a 60%, #2563eb 100%);
  border-radius: 18px;
  box-shadow: 0 6px 32px 0 rgba(37,99,235,0.10), 0 1.5px 6px 0 rgba(0,0,0,0.10);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  position: relative;
  padding: 18px 8px 8px 8px;
  box-sizing: border-box;
}

.gauge-chart {
  min-height: 260px;
  width: 100%;
}

.gauge-title {
  position: absolute;
  bottom: 18px;
  left: 0;
  width: 100%;
  text-align: center;
  color: #a5b4fc;
  font-size: 1.1rem;
  font-weight: 600;
  letter-spacing: 0.01em;
  font-family: 'Segoe UI', 'Roboto', 'Arial', sans-serif;
  pointer-events: none;
  user-select: none;
}
</style>
