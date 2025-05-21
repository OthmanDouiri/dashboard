<template>
  <ion-page>
    <HeaderComponent title="📈 Técnico" />

    <ion-content :fullscreen="true" class="ion-padding">
      <!-- Grid principal del Dashboard -->
      <ion-grid class="dashboard-grid">
        <!-- 🟢 Fila 1: 4 Columnas -->
        <ion-row class="ion-row-1">
          <ion-col size="12" size-lg="4">
            <div class="box">
              <spark-line v-bind="sparkData1"/>
            </div>
          </ion-col>
          <ion-col size="6" size-lg="4">
            <div class="box">
              <spark-line v-bind="sparkData2"/>
            </div>
          </ion-col>
          <ion-col size="6" size-lg="4">
            <div class="box">
              <spark-line v-bind="sparkData3"/>
            </div>
          </ion-col>
        </ion-row>


        <!-- 🔵 Fila 2: 2 Columnas -->
        <ion-row class="ion-row-2">
          <ion-col size="12" size-md="3" push-md="9">
            <div class="box">
              <EchartsGauge :value="currentValue" title="USUARIOS ONLINE" />
            </div>
          </ion-col>
            <ion-col size="12" size-md="9" pull-md="3">
              <div class="box">
                <ApexLineRT :series="series" title="Usuarios online" :kpi-target="70" color="#3b82f6" />
              </div>
            </ion-col>
        </ion-row>


        <!-- 🟠 Fila 3: 2 Columnas -->
        <ion-row class="ion-row-3">
          <ion-col size="12" size-lg="4.5">
            <div class="box">
              <ChartJSLineAreaRT chartType="line" title="Carga CPU" color="#10b981" />
            </div>
          </ion-col>
          <ion-col size="12" size-lg="4.5">
            <div class="box">
              <ChartJSLineAreaRT chartType="area" title="Memoria" color="#3b82f6" :min="50" :max="70" />
            </div>
          </ion-col>
          <ion-col size="12" size-lg="3">
            <div class="box">
              <EchartsGaugeMultiple :segments="ringSegments" />
            </div>
          </ion-col>
        </ion-row>
      </ion-grid>
    </ion-content>
  </ion-page>
</template>


<script setup lang="ts">
/***** 🧰 DEPENDENCIAS ******/


import HeaderComponent from '@/components/HeaderComponent.vue';
import { IonContent, IonPage, IonGrid, IonRow, IonCol } from '@ionic/vue';
import { ref, onMounted, onUnmounted } from 'vue';


// Sparkline
import SparkLine from '@/components/SparkLine.vue';


// ApexLineChart Realtime
import ApexLineRT from '@/components/ApexLineRT.vue';


// Echarts
import EchartsGauge from '@/components/EchartsGauge.vue';

// Echarts - Múltiple
// Ensure the file exists in the specified path or correct the path if necessary
import EchartsGaugeMultiple from '@/components/EchartsGaugemultiple.vue';
// ChartJS
import ChartJSLineAreaRT from '@/components/ChartJSLineAreaRT.vue';


/***** 🛠️ CONSTANTES y VARIABLES *****/


// Constantes
const UPDATE_INTERVAL = 1000;
const MAX_DATA_POINTS = 60;   // Límitar nº puntos del gráfico para mejor rendimiento (ApexLineRT)


// Variables
let lastDate = Date.now();
let interval: ReturnType<typeof setInterval>;


/***** 🗃️ DATA *****/


// SparkLine - CONVERSIONS
const sparkData1 = ref({
  title: "CONVERSIONS",                // Título nuevo
  value: "894",                        // Nuevo valor principal
  bgColor: "gradient-green",          // Cambiado a un gradiente verde
  textColor: "white",
  iconName: "checkmark-done-outline", // Nuevo ícono
  chartOptions: {
    chart: {
      id: 'conversions',
      type: 'area',
      sparkline: { enabled: true },
      dropShadow: { enabled: true, top: 2, left: 0, blur: 3, opacity: 0.3 }
    },
    stroke: { curve: 'smooth', width: 2 },
    colors: ['#10B981'], // Verde
    tooltip: {
      theme: 'dark',
      x: { show: false },
      y: { title: { formatter: () => '' } }
    }
  },
  chartSeries: [{ data: [12, 18, 26, 32, 48, 61, 72, 83, 77, 90] }]
});

// SparkLine - SIGNUPS
const sparkData2 = ref({
  title: "SIGNUPS",
  value: "1543",
  bgColor: "gradient-blue",  // Nuevo color
  textColor: "white",
  iconName: "person-add-outline",     // Ícono representativo
  chartOptions: {
    chart: {
      id: 'signups',
      type: 'bar',
      sparkline: { enabled: true },
      dropShadow: { enabled: true, top: 1, left: 1, blur: 2, opacity: 0.4 }
    },
    stroke: { curve: 'smooth', width: 3 },
    colors: ['#8B5CF6'], // Morado
    tooltip: {
      theme: 'dark',
      x: { show: false },
      y: { title: { formatter: () => '' } }
    }
  },
  chartSeries: [{ data: [5, 8, 14, 22, 31, 40, 58, 73, 91, 110] }]
});

// SparkLine - ERRORS
const sparkData3 = ref({
  title: "ERRORS",
  value: "37",
  bgColor: "gradient-orange",
  textColor: "white",
  iconName: "alert-circle-outline",
  chartOptions: {
    chart: {
      id: 'errors',
      type: 'line',
      sparkline: { enabled: true },
      dropShadow: { enabled: true, top: 1, left: 1, blur: 2, opacity: 0.6 }
    },
    stroke: { curve: 'stepline', width: 2 }, // Línea escalonada
    colors: ['#EF4444'], // Rojo intenso
    tooltip: {
      theme: 'dark',
      x: { show: false },
      y: { title: { formatter: () => '' } }
    }
  },
  chartSeries: [{ data: [1, 3, 2, 6, 7, 5, 10, 8, 4, 2] }]
});


// Data: ApexLineRT - Series
const data = ref<{ x: number; y: number }[]>([
  { x: Date.now() - 10000, y: 45 },
  { x: Date.now() - 8000, y: 52 },
  { x: Date.now() - 6000, y: 48 },
  { x: Date.now() - 4000, y: 65 },
  { x: Date.now() - 2000, y: 59 },
  { x: Date.now(), y: 63 }
]);
const series = ref([{ name: 'Usuarios', data: data.value }]);


// Data: ECharts - Gauge Simple: Valor inicial
const currentValue = ref(0);  


// Data: Echarts - Gauge Múltiple: Valores iniciales
const ringSegments = ref([
  { value: 0, name: '🥘 Marruecos', color: '#f97316', min: 80, max: 99 },
  { value: 0, name: '🌍 Mundo', color: '#10b981', min: 10, max: 30 }
]);



/***** 🧠 LÓGICA REALTIME *****/
function addDataRealTime() {
 
 const newX = lastDate + UPDATE_INTERVAL;
 const newY = Math.floor(Math.random() * 90) + 10;
 data.value.push({ x: newX, y: newY });


 // ApexLineRT - Cuidar uso de memoria
 if (data.value.length > MAX_DATA_POINTS) {
   data.value = data.value.slice(-2);
   series.value = [{ name: 'Usuarios', data: data.value }]; // Asegúra que ApexCharts recoja el cambio
 }

 lastDate = newX;


 // ECharts - Gauge simple: nuevo valor
 currentValue.value = newY;


 // Echarts - Gauge múltiple: nuevo valor para cada segmento
 ringSegments.value.forEach((s) => {
   s.value = Math.floor(Math.random() * (s.max - s.min + 1)) + s.min;
 });
}


/***** 🔄 CICLO DE VIDA *****/
onMounted(() => {
  interval = setInterval(addDataRealTime, UPDATE_INTERVAL);
});


onUnmounted(() => {
  clearInterval(interval);
});



</script>


<style scoped>


ion-row{
  overflow: hidden;
}


ion-col {
  max-height: 100%;
  --ion-grid-column-padding:10px;
}


/* El contenido real de cada columna */
.box {
  background: #ffffff;
  box-shadow: 0 0 10px rgba(197, 191, 191, 0.884);
  height: 100%;
  max-height: 100%;
  overflow: hidden;
  border-radius:5px;
  display: flex;
  justify-content: center;
  align-items: start;
}


/* Aplicar altura total y por filas, solo en pantallas ≥ md */
@media (min-width: 992px) {  
  ion-grid{height: 100%;}
  .ion-row-1{height: 20%; max-height: 20%;}
  .ion-row-2{height: 40%; max-height: 40%;}
  .ion-row-3{height: 40%; max-height: 40%;}
}


</style>
