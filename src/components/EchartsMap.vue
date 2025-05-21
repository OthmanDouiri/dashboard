<template>
    <VEChart class="map-chart" :option="mapOptions" autoresize />
  </template>
  
  <script setup lang="ts">
  import { ref, watchEffect, onMounted } from 'vue';
  
  // ECharts Core
  import VEChart from "vue-echarts";
  import * as echarts from "echarts/core";
  import { MapChart } from 'echarts/charts';
  import { CanvasRenderer } from 'echarts/renderers';
  import {
    TooltipComponent,
    VisualMapComponent,
    TitleComponent,
    ToolboxComponent
  } from 'echarts/components';
  
  // GeoMap JSON
  import europeMap from "@/assets/europe.geo.json";
  
  // Register necessary modules
  echarts.use([
    MapChart,
    CanvasRenderer,
    TooltipComponent,
    VisualMapComponent,
    TitleComponent,
    ToolboxComponent
  ]);
  
  // Register Europe map with Iceland manually placed
  onMounted(() => {
    echarts.registerMap("europe", europeMap as any, {
      Iceland: {
        left: -20,
        top: 65,
        width: 12
      }
    });
  });
  
  // Props with defaults
  const props = withDefaults(defineProps<{
    title?: string;
    subtitle?: string;
    data: { name: string; value: number }[];
  }>(), {
    title: 'Título del gráfico',
    subtitle: 'Subtítulo',
    data: () => []
  });
  
  // Reactive map config
  const mapOptions = ref({});
  
  watchEffect(() => {
    mapOptions.value = {
      title: {
        text: props.title,
        subtext: props.subtitle,
        left: '0%', 
        top: '0%',
        
        textStyle: {
          color: "#ffffff",
          fontSize: 16,
          fontWeight: "bold"
        },
        subtextStyle: {
          color: "#8C8C8C",
        },
      },
      toolbox: {
        show: true,
        left: 'right',
        top: 'top',
        feature: {
          dataView: {
            readOnly: false,
            backgroundColor: "#1E1E1E",
            textareaColor: "#1E1E1E",
            textColor: "#8C8C8C",
            buttonColor: "#071c49",
            lang: ['Datos', 'Cerrar', 'Actualizar'],
          },
          restore: {},
          saveAsImage: {}
        }
      },
      tooltip: {
        trigger: "item",
      },
      visualMap: {
        min: 3000,
        max: 16000,
        left: "left",
        top: "bottom",
        text: ["Más", "Menos"],
        textStyle: { color: "#B9B8CE" },
        calculable: true,
        inRange: { color: ["#e0f3f8", "#74add1", "#4575b4"] },
        outOfRange: {
          color: ['rgba(200, 200, 200, 0.2)']
        }
      },
      series: [
        {
          name: "Descargas",
          type: "map",
          map: "europe",
          roam: true,
          emphasis: {
            label: { show: true },
            itemStyle: {
              areaColor: '#74add1',
              borderWidth: 1
            }
          },
          itemStyle: {
            areaColor: 'rgba(128, 128, 128, 0.5)',
            borderColor: 'rgba(255, 255, 255, 0.5)',
            borderWidth: 0.5
          },
          data: props.data,
        },
      ],
    };
  });
  </script>
  
  <style scoped>
  .map-chart {
    width: 100%;
    min-height: 400px;
    max-height: 600px;
    background-color: #1E1E1E;
    border-radius: 8px;
    padding: 45px;
  }
  </style>
  