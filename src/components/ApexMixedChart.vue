
<template>
  <div class="box-mixedChart">
    <VueApexCharts type="line" height="100%" :options="options" :series="series" />
  </div>
</template>


<script setup lang="ts">

import VueApexCharts from 'vue3-apexcharts';
import { withDefaults, defineProps } from 'vue';




// eslint-disable-next-line @typescript-eslint/no-unused-vars
const props = withDefaults(defineProps<{
  series: ApexAxisChartSeries;
  options?: ApexCharts.ApexOptions;
}>(), {
  options: () => ({
    chart: {
      type: 'line',
      stacked: false,
      background: 'transparent',
    },
    theme: {
      mode: 'dark',
      palette: 'palette1',
    },
    title: {
      text: 'Mixed Chart',
      align: 'left',
      style: {
        fontSize: '16px',
        fontWeight: 'bold',
        color: '#8C8C8C',
      },
    },
    stroke: {
      width: [0, 2, 4],
      curve: 'smooth'
    },
    plotOptions: {
      bar: {
        columnWidth: '50%'
      }
    },
    fill: {
      opacity: [0.85, 0.25, 1],
      gradient: {
        inverseColors: false,
        shade: 'light',
        type: 'vertical',
        opacityFrom: 0.85,
        opacityTo: 0.55,
        stops: [0, 100, 100, 100],
      }
    },
    labels: [
      '01/01/2024', '02/01/2024', '03/01/2024',
      '04/01/2024', '05/01/2024', '06/01/2024',
      '07/01/2024', '08/01/2024', '09/01/2024',
      '10/01/2024', '11/01/2024'
    ],
    markers: { size: 0 },
    xaxis: {
      type: 'datetime',
      labels: { style: { colors: '#8C8C8C' } }
    },
    yaxis: {
      labels: { style: { colors: '#8C8C8C' } },
      title: {
        style: { color: '#8C8C8C', fontWeight: 'normal' }
      },
      tooltip: { enabled: true }
    },
    grid: {
      show: true,
      borderColor: '#555',
    },
    legend: {
      position: 'top'
    },
    tooltip: {
      shared: true,
      theme: 'dark',
      intersect: false,
      y: {
        formatter: (y: number | null) => y !== null ? `${y.toFixed(0)} points` : ''
      }
    }
  })
});


</script>


<style scoped>
  .box-mixedChart{
    display: flex;
    flex-direction: column;
    height: 100%;
    min-height: 240px;
    width: 100%;
    padding: 16px;
  }
</style>
