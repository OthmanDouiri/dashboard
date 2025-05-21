<template>
    <div class="apexline-container">
      <div class="chart-header">
        <h3 class="chart-title">{{ title }}</h3>
        <div class="kpi-info">
          <span class="kpi-label">Meta:</span>
          <span class="kpi-value">{{ kpiTarget }}%</span>
        </div>
      </div>
      <div class="chart-wrapper">
        <apexchart
          type="line"
          :height="300"
          :options="chartOptions"
          :series="series"
        />
      </div>
    </div>
  </template>
  
  <script setup lang="ts">
  import { computed } from 'vue';
  import VueApexCharts from 'vue3-apexcharts';
  
  // Register the component locally
  defineExpose({
    components: {
      apexchart: VueApexCharts
    }
  });
  
  // Props definition with destructuring for <script setup>
  const { series, title, kpiTarget, color } = defineProps<{
    series: any[]
    title?: string
    kpiTarget?: number
    color?: string
  }>();
  
  // Computed chart options
  const chartOptions = computed(() => ({
    chart: {
      id: 'realtime-line',
      height: 300,
      type: 'line',
      animations: {
        enabled: true,
        easing: 'linear',
        dynamicAnimation: {
          speed: 1000
        }
      },
      toolbar: {
        show: false
      },
      zoom: {
        enabled: false
      },
      background: '#fff',
      fontFamily: 'inherit'
    },
    colors: [color],
    dataLabels: {
      enabled: false
    },
    stroke: {
      curve: 'smooth',
      width: 3
    },
    title: {
      show: false
    },
    markers: {
      size: 0
    },
    xaxis: {
      type: 'datetime',
      labels: {
        format: 'HH:mm:ss',
      },
      axisBorder: {
        show: false
      },
      axisTicks: {
        show: false
      }
    },
    yaxis: {
      min: 0,
      max: 100,
      labels: {
        formatter: (value: number): string => `${Math.round(value)}%`
      }
    },
    legend: {
      show: false
    },
    tooltip: {
      x: {
        format: 'HH:mm:ss'
      },
      y: {
        formatter: (value: number) => `${Math.round(value)}%`
      }
    },
    grid: {
      borderColor: '#f1f1f1',
      padding: {
        top: 10,
        right: 10,
        bottom: 10,
        left: 10
      }
    },
    annotations: {
      yaxis: [
        {
          y: kpiTarget,
          borderColor: '#FF4560',
          label: {
            borderColor: '#FF4560',
            style: {
              color: '#fff',
              background: '#FF4560'
            },
            text: 'KPI Target'
          }
        }
      ]
    }
  }));
  </script>
  
  <style scoped>
  .apexline-container {
    display: flex;
    flex-direction: column;
    width: 100%;
    height: 100%;
    padding: 10px;
  }
  
  .chart-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 10px;
  }
  
  .chart-title {
    font-size: 1.1rem;
    font-weight: 600;
    color: #374151;
    margin: 0;
  }
  
  .kpi-info {
    display: flex;
    align-items: center;
    gap: 5px;
  }
  
  .kpi-label {
    font-size: 0.9rem;
    color: #6b7280;
  }
  
  .kpi-value {
    font-size: 0.9rem;
    font-weight: 600;
    color: #FF4560;
  }
  
  .chart-wrapper {
    flex: 1;
    min-height: 300px;
    width: 100%;
    position: relative;
  }
  
  :deep(.apexcharts-canvas),
  :deep(.apexcharts-svg) {
    width: 100% !important;
    height: 100% !important;
  }
  </style>
  