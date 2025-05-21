<template>
  <ion-page>
    <HeaderComponent title="🚀 Negocio" />

    <ion-content :fullscreen="true" class="ion-padding">
      <!-- Grid principal del Dashboard -->
      <ion-grid class="dashboard-grid">
        <!-- 🟢 Fila 1: 4 Columnas -->
        <ion-row class="ion-row-1">
          <ion-col size="6" size-lg="3">
            <div class="box">
              <spark-line v-bind="sparkData1"/>
            </div>
          </ion-col>
          <ion-col size="6" size-lg="3">
            <div class="box">
              <spark-line v-bind="sparkData2"/>
            </div>
          </ion-col>
          <ion-col size="6" size-lg="3">
            <div class="box">
              <spark-line v-bind="sparkData3"/>

            </div>
          </ion-col>
          <ion-col size="6" size-lg="3">
            <div class="box">
              <spark-line v-bind="sparkData4"/>

            </div>
          </ion-col>
        </ion-row>


        <!-- 🔵 Fila 2: 2 Columnas -->
        <ion-row class="ion-row-2">
          <ion-col size="12" size-lg="9">
            <div class="box">
              <ApexMixedChart :series="dataMixedChartSeries" />
            </div>
          </ion-col>
          <ion-col size="12" size-lg="3">
            <div class="box">
              <EchartsGauge :value=dataGauge title="DESCARGAS" />
            </div>
          </ion-col>
        </ion-row>


        <!-- 🟠 Fila 3: 2 Columnas -->
        <ion-row class="ion-row-3">
          <ion-col size="12" size-lg="6">
            <div class="box">
              <EchartsMap :data=dataDownloads title="Descargas" subtitle="KPI: 8 Países > 1K" />
            </div>
          </ion-col>
          <ion-col size="12" size-lg="6">
            <div class="box">
                <NeavyGoose/>
            </div>
          </ion-col>
        </ion-row>
      </ion-grid>
    </ion-content>
  </ion-page>
</template>


<script setup lang="ts">
import HeaderComponent from '@/components/HeaderComponent.vue';
import NeavyGoose from '@/components/NeavyGoose.vue';
import EchartsMap from '@/components/EchartsMap.vue';
import EchartsGauge from '@/components/EchartsGauge.vue';
import { IonContent, IonPage, IonGrid, IonRow, IonCol } from '@ionic/vue';
import { ref } from 'vue';
import SparkLine from '@/components/SparkLine.vue';
import ApexMixedChart from '@/components/ApexMixedChart.vue';


const sparkData1 = ref({
  /* Propiedades del componente */
  title: "CLICKS",
  value: "84685",
  bgColor: "gradient-blue",
  textColor: "white",
  iconName: "navigate-outline",
  /* Propiedades del componentes interno de ApexChart */
  chartOptions: {
    chart: {
      id: 'clicks',
      type: 'area',
      sparkline: { enabled: true },
      dropShadow: { enabled: true, top: 1, left: 1, blur: 2, opacity: 0.5 }
    },
    stroke: { curve: 'smooth', width: 3 },
    colors: ['#fff'],
    tooltip: { theme: 'dark', x: { show: false }, y: { title: { formatter: () => '' } } }
  },
  chartSeries: [{ data: [25, 66, 41, 59, 25, 44, 12, 36, 9, 21] }],
});

const sparkData2 = ref({
  /* Propiedades del componente */
  title: "VIEWS",
  value: "7542",
  bgColor: "gradient-pink",
  textColor: "white",
  iconName: "eye-outline",
  /* Propiedades del componentes interno de ApexChart */
  chartOptions: {
    chart: {
      id: 'views',
      type: 'bar',
      sparkline: { enabled: true },
      dropShadow: { enabled: true, top: 1, left: 1, blur: 2, opacity: 0.5 }
    },
    stroke: { curve: 'smooth', width: 3 },
    colors: ['#fff'],
    tooltip: { theme: 'dark', x: { show: false }, y: { title: { formatter: () => '' } } }
  },
  chartSeries: [{ data: [25, 66, 41, 59, 25, 44, 12, 36, 9, 21] }],
});


const sparkData3 = ref({
  /* Propiedades del componente */
  title: "LEADS",
  value: "2024",
  bgColor: "gradient-orange",
  textColor: "white",
  iconName: "people-outline",
  /* Propiedades del componentes interno de ApexChart */
  chartOptions: {
    chart: {
      id: 'leads',
      type: 'line',
      sparkline: { enabled: true },
      dropShadow: { enabled: true, top: 1, left: 1, blur: 2, opacity: 0.5 }
    },
    stroke: { curve: 'smooth', width: 3 },
    colors: ['#fff'],
    tooltip: { theme: 'dark', x: { show: false }, y: { title: { formatter: () => '' } } }
  },
  chartSeries: [{ data: [25, 66, 41, 59, 25, 44, 12, 36, 9, 21] }],
});

// sells 674k euros
const sparkData4 = ref({
  /* Propiedades del componente */
  title: "VENDAS",
  value: "374K",
  bgColor: "gradient-green",
  textColor: "white",
  iconName: "cash-outline",
  /* Propiedades del componentes interno de ApexChart */
  chartOptions: {
    chart: {
      id: 'sells',
      type: 'line',
      sparkline: { enabled: true },
      dropShadow: { enabled: true, top: 1, left: 1, blur: 2, opacity: 0.5 }
    },
    stroke: { curve: 'smooth', width: 3 },
    colors: ['#fff'],
    tooltip: { theme: 'dark', x: { show: false }, y: { title: { formatter: () => '' } } }
  },
  chartSeries: [{ data: [25, 66, 41, 59, 25, 44, 12, 36, 9, 21] }],
});

// 📊 Data: ApexMixedChart
const dataMixedChartSeries = ref([
  {
    name: 'Visitas (miles)',
    type: 'column',
    data: [23, 11, 22, 27, 13, 22, 37, 21, 44, 22, 30]
  },
  {
    name: 'Ingresos (€)',
    type: 'area',
    data: [44, 55, 41, 67, 22, 43, 21, 41, 56, 27, 43]
  },
  {
    name: 'Actividades (1-5)',
    type: 'line',
    data: [30, 25, 36, 30, 45, 35, 64, 52, 59, 36, 39]
  }
]);


// 🔢 Data: Gauge Value
const dataGauge = ref(40); // Example value




// 🌎 Data: EchartsMao - GEOMAPS
const dataDownloads = ref([
  { name: "United States", value: 25000 },
  { name: "Brazil", value: 18000 },
  { name: "India", value: 22000 },
  { name: "China", value: 20000 },
  { name: "Germany", value: 16000 },
  { name: "France", value: 15000 },
  { name: "United Kingdom", value: 17000 },
  { name: "Japan", value: 13000 },
  { name: "Italy", value: 11000 },
  { name: "Spain", value: 9000 },
  { name: "Mexico", value: 8000 },
  { name: "Russia", value: 7000 },
  { name: "Netherlands", value: 6000 },
  { name: "Turkey", value: 5000 },
  { name: "Saudi Arabia", value: 4000 },
  { name: "South Korea", value: 3000 },
  { name: "Australia", value: 14000 },
  { name: "Canada", value: 12000 },
  { name: "South Africa", value: 9000 },
]);



</script>


<style scoped>


ion-row{
  overflow: hidden;
  /*border: 1px solid red;*/
}


ion-col {
  max-height: 100%;
  --ion-grid-column-padding:10px;
  /*background-color: blue;*/
}


/* El contenido real de cada columna */
.box {
  background: #2b2842;
  box-shadow: 0 0 10px rgba(197, 191, 191, 0.884);
  height: 100%;
  max-height: 100%;
  overflow: hidden;
  border-radius:5px;
  display: flex;  
  flex-direction: column;
  justify-content: center;
  align-items: center;
  
}


/* Aplicar altura total y por filas, solo en pantallas ≥ md */
@media (min-width: 992px) {  
  ion-grid{height: 100%;}
  .ion-row-1{height: 20%; max-height: 20%;}
  .ion-row-2{height: 40%; max-height: 40%;}
  .ion-row-3{height: 40%; max-height: 40%;}
}
</style>

