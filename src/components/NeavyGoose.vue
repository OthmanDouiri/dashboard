<template>
  <div class="tourism-chart-wrapper">
    <div class="chart-header">
      <h2>Turismo en Marruecos <span class="year">(2025)</span></h2>
    </div>
    <div class="chart-body">
      <Bar :data="chartData" :options="chartOptions" />
    </div>
    <p class="chart-footer">
      * Datos aproximados basados en estadísticas oficiales de turismo en Marruecos.
    </p>
  </div>
</template>

<script setup>
import {
  Chart as ChartJS,
  Title,
  Tooltip,
  Legend,
  BarElement,
  CategoryScale,
  LinearScale
} from 'chart.js'
import { Bar } from 'vue-chartjs'

ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale)

const chartData = {
  labels: ['Marrakech', 'Casablanca', 'Agadir', 'Fez', 'Tánger', 'Rabat'],
  datasets: [
    {
      label: 'Visitantes (millones)',
      backgroundColor: [
        '#2563eb', '#1d4ed8', '#0ea5e9', '#06b6d4', '#10b981', '#22d3ee'
      ],
      borderRadius: 12,
      borderSkipped: false,
      data: [3.2, 2.5, 1.8, 1.4, 1.2, 1.1]
    }
  ]
}

const chartOptions = {
  responsive: true,
  maintainAspectRatio: false,
  animation: {
    duration: 1200,
    easing: 'easeOutQuart'
  },
  scales: {
    y: {
      beginAtZero: true,
      title: {
        display: true,
        text: 'Millones de visitantes',
        color: '#fff',
        font: { weight: 'bold', size: 14 }
      },
      ticks: { color: '#e0e7ef', font: { size: 13 } },
      grid: { color: 'rgba(255,255,255,0.08)' }
    },
    x: {
      title: {
        display: true,
        text: 'Ciudades',
        color: '#fff',
        font: { weight: 'bold', size: 14 }
      },
      ticks: { color: '#e0e7ef', font: { size: 13 } },
      grid: { display: false }
    }
  },
  plugins: {
    legend: {
      display: false
    },
    tooltip: {
      backgroundColor: '#23213a',
      titleColor: '#fff',
      bodyColor: '#fff',
      borderColor: '#2563eb',
      borderWidth: 1
    }
  }
}
</script>

<style scoped>
.tourism-chart-wrapper {
  width: 100%;
  height: 100%;
  min-height: 340px;
  background: linear-gradient(135deg, #23213a 60%, #2563eb 100%);
  border-radius: 18px;
  box-shadow: 0 6px 32px 0 rgba(37,99,235,0.10), 0 1.5px 6px 0 rgba(0,0,0,0.10);
  display: flex;
  flex-direction: column;
  justify-content: stretch;
  align-items: stretch;
  padding: 24px 18px 12px 18px;
  box-sizing: border-box;
}

.chart-header {
  text-align: center;
  margin-bottom: 10px;
}
.chart-header h2 {
  color: #fff;
  font-size: 1.5rem;
  font-weight: 700;
  letter-spacing: 0.01em;
  margin: 0;
}
.chart-header .year {
  color: #a5b4fc;
  font-size: 1rem;
  font-weight: 400;
  margin-left: 8px;
}

.chart-body {
  flex: 1 1 0;
  min-height: 220px;
  max-height: 100%;
  position: relative;
}

.chart-footer {
  margin-top: 10px;
  font-size: 0.95rem;
  color: #cbd5e1;
  text-align: center;
  opacity: 0.85;
}
</style>
