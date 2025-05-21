<template>
  <div class="chartjs-modern-container">
    <div class="chartjs-header">
      <h3 class="chartjs-title">{{ title }}</h3>
    </div>
    <div class="chartjs-canvas">
      <canvas ref="chartCanvas"></canvas>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted, computed } from 'vue';
import Chart, { ChartOptions } from 'chart.js/auto';
import { LinearScale, CategoryScale, PointElement, LineElement, Filler, Tooltip, Legend } from 'chart.js';

Chart.register(LinearScale, CategoryScale, PointElement, LineElement, Filler, Tooltip, Legend);

const props = defineProps({
  chartType: { type: String, default: 'line', validator: (v: string) => ['line', 'area'].includes(v) },
  title: { type: String, default: 'Chart' },
  color: { type: String, default: '#3b82f6' },
  min: { type: Number, default: 0 },
  max: { type: Number, default: 100 }
});

const chartCanvas = ref<HTMLCanvasElement | null>(null);
let chart: Chart | null = null;
const chartData = ref<number[]>([]);
const chartLabels = ref<string[]>([]);
const MAX_DATA_POINTS = 60;
let updateInterval: ReturnType<typeof setInterval> | null = null;
let gradientCache: CanvasGradient | null = null;
const isTabActive = ref(!document.hidden);

const handleVisibilityChange = () => {
  isTabActive.value = !document.hidden;
};

const getGradient = (ctx: CanvasRenderingContext2D) => {
  if (!gradientCache || ctx.canvas.height !== (gradientCache as any).height) {
    gradientCache = ctx.createLinearGradient(0, 0, 0, ctx.canvas.height);
    gradientCache.addColorStop(0, `${props.color}CC`);
    gradientCache.addColorStop(1, `${props.color}10`);
    (gradientCache as any).height = ctx.canvas.height;
  }
  return gradientCache;
};

const chartOptions = computed<ChartOptions<'line'>>(() => ({
  responsive: true,
  maintainAspectRatio: false,
  animation: { duration: 800, easing: 'easeOutQuad' },
  transitions: {
    show: {
      animations: {
        x: { from: 0 },
        y: { from: 'bottom' }
      }
    }
  },
  scales: {
    y: { beginAtZero: true, min: props.min, max: props.max, grid: { color: 'rgba(200, 200, 200, 0.2)' } },
    x: { grid: { display: false }, ticks: { maxRotation: 0, maxTicksLimit: 6 } }
  },
  plugins: {
    legend: { display: false },
    tooltip: { backgroundColor: 'rgba(0, 0, 0, 0.7)', displayColors: false }
  },
  elements: {
    line: { tension: 0.3 },
    point: { radius: 1, hoverRadius: 3 }
  },
  interaction: { mode: 'nearest', intersect: false }
}));

const initChart = () => {
  if (!chartCanvas.value) return;
  
  const ctx = chartCanvas.value.getContext('2d');
  if (!ctx) return;

  chartData.value = Array.from({ length: 10 }, () => 
    Math.floor(Math.random() * (props.max - props.min)) + props.min
  );
  
  chartLabels.value = Array.from({ length: 10 }, (_, i) => {
    const now = new Date();
    now.setSeconds(now.getSeconds() - (10 - i));
    return now.toLocaleTimeString('es-ES', { hour: '2-digit', minute: '2-digit', second: '2-digit' });
  });

  chart = new Chart(ctx, {
    type: 'line',
    data: {
      labels: chartLabels.value,
      datasets: [{
        label: props.title,
        data: chartData.value,
        borderColor: props.color,
        backgroundColor: props.chartType === 'area' ? getGradient(ctx) : 'transparent',
        fill: props.chartType === 'area',
        borderWidth: 2
      }]
    },
    options: chartOptions.value
  });
};

const updateChart = () => {
  if (!chart || !chartCanvas.value) return;
  
  requestAnimationFrame(() => {
    if (!chart || !chartCanvas.value) return;
    const ctx = chartCanvas.value.getContext('2d');
    if (!ctx) return;

    const value = Math.floor(Math.random() * (props.max - props.min)) + props.min;
    const now = new Date();
    const timeStr = now.toLocaleTimeString('es-ES', { hour: '2-digit', minute: '2-digit', second: '2-digit' });

    chart.data.labels = [...chartLabels.value.slice(-MAX_DATA_POINTS + 1), timeStr];
    chart.data.datasets[0].data = [...chartData.value.slice(-MAX_DATA_POINTS + 1), value];

    if (props.chartType === 'area') {
      chart.data.datasets[0].backgroundColor = getGradient(ctx);
    }

    chart.update();

    chartLabels.value = chart.data.labels as string[];
    chartData.value = chart.data.datasets[0].data as number[];
  });
};

let resizeObserver: ResizeObserver | null = null;

onMounted(() => {
  document.addEventListener('visibilitychange', handleVisibilityChange);
  initChart();
  updateInterval = setInterval(() => isTabActive.value && updateChart(), 1000);

  resizeObserver = new ResizeObserver(() => chart?.resize());
  if (chartCanvas.value?.parentElement) {
    resizeObserver.observe(chartCanvas.value.parentElement);
  }
});

onUnmounted(() => {
  if (chart) chart.destroy();
  if (updateInterval) clearInterval(updateInterval);
  if (resizeObserver) resizeObserver.disconnect();
  document.removeEventListener('visibilitychange', handleVisibilityChange);
});
</script>

<style scoped>
.chartjs-modern-container {
  width: 100%;
  height: 100%;
  min-height: 260px;
  background: linear-gradient(135deg, #23213a 60%, #2563eb 100%);
  border-radius: 18px;
  box-shadow: 0 6px 32px 0 rgba(37,99,235,0.10), 0 1.5px 6px 0 rgba(0,0,0,0.10);
  display: flex;
  flex-direction: column;
  justify-content: stretch;
  align-items: stretch;
  padding: 20px 18px 12px 18px;
  box-sizing: border-box;
}

.chartjs-header {
  text-align: left;
  margin-bottom: 10px;
  padding-left: 2px;
}

.chartjs-title {
  color: #fff;
  font-size: 1.25rem;
  font-weight: 700;
  letter-spacing: 0.01em;
  margin: 0;
  font-family: 'Segoe UI', 'Roboto', 'Arial', sans-serif;
}

.chartjs-canvas {
  flex: 1 1 0;
  min-height: 180px;
  max-height: 100%;
  position: relative;
  width: 100%;
}

canvas {
  position: absolute;
  top: 0;
  left: 0;
  width: 100% !important;
  height: 100% !important;
  border-radius: 12px;
  background: transparent;
}
</style>
