<template>
  <ion-page>
    <HeaderComponent title="🎯 KPIs" />

    <ion-content :fullscreen="true" class="ion-padding">
      <h1 class="ion-padding">🚀 KPIs de Negocio</h1>
      <ion-accordion-group expand="inset" :multiple="true">
        <ion-accordion v-for="item in smartGoals" :key="item.id" :value="item.id.toString()">
          <ion-item slot="header">
            <ion-label>{{ item.id }}. {{ item.title }}</ion-label>
          </ion-item>
          <div class="ion-padding" slot="content">
            <p>{{ item.description }}</p>
            <ion-list :inset="true">
              <ion-item v-for="(element, index) in item.smart" :key="index">
                <ion-label><b>{{ element.letter }}</b> → {{ element.content }}</ion-label>
              </ion-item>
            </ion-list>
          </div>
        </ion-accordion>
      </ion-accordion-group>
      <br>




      <h1 class="ion-padding">📈 KPIs Técnicos</h1>
      <ion-accordion-group expand="inset" :multiple="true">
        <ion-accordion v-for="item in technicalKpis" :key="item.id" :value="item.id.toString()">
          <ion-item slot="header">
            <ion-label>{{ item.id }}. {{ item.title }}</ion-label>
          </ion-item>
          <div class="ion-padding" slot="content">
            <p>{{ item.description }}</p>
            <ion-list :inset="true">
              <ion-item v-for="(element, index) in item.smart" :key="index">
                <ion-label><b>{{ element.letter }}</b> → {{ element.content }}</ion-label>
              </ion-item>
            </ion-list>
          </div>
        </ion-accordion>
      </ion-accordion-group>
    </ion-content>
  </ion-page>
</template>


<script setup lang="ts">
import HeaderComponent from '@/components/HeaderComponent.vue';
import { IonContent, IonPage, IonAccordionGroup, IonAccordion, IonItem, IonLabel, IonList } from '@ionic/vue';
import { ref } from 'vue';


// Definición de la estructura de datos para un objetivo SMART
interface SmartElement {
  letter: string;
  content: string;
}


interface SmartGoal {
  id: number;
  title: string;
  description: string;
  smart: SmartElement[];
}


// Array de KPIs de negocio
const smartGoals = ref<SmartGoal[]>([
  {
  id: 1,
  title: "Incrementar clics en el sitio",
  description: "Incrementar los clics en el sitio web en un 20% (de 70,571 a 84,685) durante los próximos 30 días mediante optimización de la navegación y botones de llamada a la acción.",
  smart: [
    { letter: "S", content: "Incrementar los clics en el sitio web en un 20%" },
    { letter: "M", content: "20% (de 70,571 a 84,685)" },
    { letter: "A", content: "optimizando la navegación y mejorando las llamadas a la acción" },
    { letter: "R", content: "para aumentar la interacción del usuario y el tiempo en el sitio" },
    { letter: "T", content: "durante los próximos 30 días" }
  ]
},
{
  id: 2,
  title: "Aumentar visualizaciones de página",
  description: "Incrementar las visualizaciones de página en un 25% (de 6,033 a 7,542) en los próximos 45 días, reforzando la estrategia SEO y compartiendo contenido en redes sociales.",
  smart: [
    { letter: "S", content: "Incrementar visualizaciones de página en un 25%" },
    { letter: "M", content: "25% (de 6,033 a 7,542)" },
    { letter: "A", content: "reforzando la estrategia SEO y redes sociales" },
    { letter: "R", content: "para mejorar el alcance orgánico y tráfico del sitio" },
    { letter: "T", content: "en los próximos 45 días" }
  ]
},
{
  id: 3,
  title: "Generar más leads calificados",
  description: "Incrementar la generación de leads en un 15% (de 1,760 a 2,024) en los próximos 60 días mediante formularios optimizados y campañas dirigidas.",
  smart: [
    { letter: "S", content: "Incrementar la generación de leads calificados en un 15%" },
    { letter: "M", content: "15% (de 1,760 a 2,024)" },
    { letter: "A", content: "mediante formularios optimizados y campañas dirigidas" },
    { letter: "R", content: "para mejorar la calidad de prospectos en el embudo de ventas" },
    { letter: "T", content: "en los próximos 60 días" }
  ]
},
{
  id: 4,
  title: "Incrementar ventas trimestrales",
  description: "Incrementar las ventas a 374K euros durante el siguiente trimestre mediante relanzamiento de productos clave y promociones exclusivas.",
  smart: [
    { letter: "S", content: "Incrementar las ventas a 374K euros" },
    { letter: "M", content: "Objetivo de 374K euros en ingresos trimestrales" },
    { letter: "A", content: "mediante relanzamiento de productos clave y promociones" },
    { letter: "R", content: "para alcanzar el objetivo financiero del trimestre" },
    { letter: "T", content: "durante el siguiente trimestre" }
  ]
},
{
  id: 5,
  title: "Mejorar la eficiencia de visitas, ingresos y actividades",
  description: "Aumentar la eficiencia en las métricas clave de visitas, ingresos y actividades en un 20% en los próximos 90 días, con el fin de optimizar las conversiones y maximizar el impacto de las campañas.",
  smart: [
    { letter: "S", content: "Aumentar las métricas clave (Visitas, Ingresos y Actividades) en un 20%" },
    { letter: "M", content: "Visitas de 22 a 26.4 miles, Ingresos de 43 a 51.6€, Actividades de 39 a 46.8" },
    { letter: "A", content: "mediante optimización de campañas de tráfico y tácticas de conversión" },
    { letter: "R", content: "para mejorar la rentabilidad de las actividades y la retención de usuarios" },
    { letter: "T", content: "en los próximos 90 días" }
  ]
},
{
  id: 6,
  title: "Mejorar índice de rendimiento general",
  description: "Incrementar el valor del indicador de rendimiento general de 40 a 60 en los próximos 45 días, mediante la optimización de procesos internos y control de calidad más estricto.",
  smart: [
    { letter: "S", content: "Incrementar el valor del índice general de rendimiento a 60" },
    { letter: "M", content: "De 40 a 60 (incremento del 50%)" },
    { letter: "A", content: "mediante optimización de procesos y control de calidad" },
    { letter: "R", content: "para mejorar la percepción del producto y la eficiencia operativa" },
    { letter: "T", content: "en los próximos 45 días" }
  ]
},
{
  id: 7,
  title: "Impulsar descargas globales",
  description: "Aumentar las descargas globales en un 25% (de un total actual de 210,000 a aproximadamente 262,500) en los próximos 3 meses mediante campañas regionales específicas y mejoras en la localización del contenido.",
  smart: [
    { letter: "S", content: "Aumentar las descargas en los principales países del mundo en un 25%" },
    { letter: "M", content: "De 210,000 a aproximadamente 262,500 descargas" },
    { letter: "A", content: "mediante campañas regionales específicas y mejoras en la localización del contenido" },
    { letter: "R", content: "para aumentar nuestra presencia global y preparar el terreno para nuevas funcionalidades multilingües" },
    { letter: "T", content: "en los próximos 3 meses" }
  ]
},
{
  id: 8,
  title: "Impulsar turismo en Marruecos",
  description: "Aumentar el número de visitantes en las principales ciudades de Marruecos en un 15% durante 2025 mediante campañas de promoción turística y mejoras en la infraestructura local.",
  smart: [
    { letter: "S", content: "Aumentar los visitantes en Marrakech, Casablanca, Agadir, Fez, Tánger y Rabat en un 15%" },
    { letter: "M", content: "De 11 millones a aproximadamente 12.65 millones de visitantes totales" },
    { letter: "A", content: "mediante campañas de promoción turística y mejoras en la infraestructura" },
    { letter: "R", content: "para posicionar Marruecos como destino turístico líder en África" },
    { letter: "T", content: "durante el año 2025" }
  ]
}

]);




// Array de KPIs técnicos
const technicalKpis = ref<SmartGoal[]>([
  {
  id: 1,
  title: "Incrementar conversiones",
  description: "Incrementar las conversiones en un 30% (de 680 a 884) en los próximos 45 días, mejorando el embudo de conversión y optimizando las llamadas a la acción.",
  smart: [
    { letter: "S", content: "Incrementar las conversiones en un 30%" },
    { letter: "M", content: "30% (de 680 a 884)" },
    { letter: "A", content: "mejorando el embudo de conversión y optimizando las llamadas a la acción" },
    { letter: "R", content: "para incrementar la tasa de conversión general del sitio" },
    { letter: "T", content: "en los próximos 45 días" }
  ]
},
{
  id: 2,
  title: "Incrementar registros",
  description: "Incrementar los registros de nuevos usuarios en un 40% (de 1,100 a 1,540) en los siguientes 60 días mediante campañas de email marketing y formularios simplificados.",
  smart: [
    { letter: "S", content: "Incrementar los registros en un 40%" },
    { letter: "M", content: "40% (de 1,100 a 1,540)" },
    { letter: "A", content: "mediante campañas de email marketing y formularios simplificados" },
    { letter: "R", content: "para aumentar la base de usuarios activos" },
    { letter: "T", content: "en los siguientes 60 días" }
  ]
},
{
  id: 3,
  title: "Reducir errores técnicos",
  description: "Reducir el número de errores técnicos reportados en un 50% (de 74 a 37) en los próximos 30 días mediante revisiones de código semanales y pruebas automatizadas.",
  smart: [
    { letter: "S", content: "Reducir los errores técnicos reportados en un 50%" },
    { letter: "M", content: "50% (de 74 a 37)" },
    { letter: "A", content: "mediante revisiones de código semanales y pruebas automatizadas" },
    { letter: "R", content: "para mejorar la estabilidad del producto" },
    { letter: "T", content: "en los próximos 30 días" }
  ]
},
{
    id: 4,
    title: "Optimizar usuarios online simultáneos",
    description: "Aumentar la capacidad de usuarios online simultáneos de 60 a 100 en los próximos 3 meses, mejorando la infraestructura del servidor y con el objetivo de soportar el aumento de tráfico en la temporada alta.",
    smart: [
      { letter: "S", content: "Aumentar la capacidad de usuarios online simultáneos" },
      { letter: "M", content: "De 60 a 100 usuarios (incremento del 66%)" },
      { letter: "A", content: "Mediante mejoras en la infraestructura del servidor" },
      { letter: "R", content: "Para soportar el aumento de tráfico en temporada alta" },
      { letter: "T", content: "En los próximos 3 meses" }
    ]
  },
  {
    id: 5,
    title: "Reducir carga de CPU",
    description: "Reducir la carga promedio de CPU de 70% a 40% en 2 meses, optimizando consultas de base de datos y procesos en segundo plano para mejorar la experiencia del usuario y reducir costos operativos.",
    smart: [
      { letter: "S", content: "Reducir la carga promedio de CPU" },
      { letter: "M", content: "Del 70% al 40% (reducción del 30%)" },
      { letter: "A", content: "Optimizando consultas de base de datos y procesos en segundo plano" },
      { letter: "R", content: "Para mejorar la experiencia del usuario y reducir costos operativos" },
      { letter: "T", content: "En 2 meses" }
    ]
  },
  {
    id: 6,
    title: "Optimizar uso de memoria",
    description: "Mantener el uso de memoria entre 50% y 65% durante el próximo trimestre, implementando mejoras en la gestión de caché y liberación de recursos para evitar fugas de memoria y mejorar estabilidad.",
    smart: [
      { letter: "S", content: "Mantener el uso de memoria en un rango óptimo" },
      { letter: "M", content: "Entre 50% y 65% (rango de 15%)" },
      { letter: "A", content: "Implementando mejoras en la gestión de caché y liberación de recursos" },
      { letter: "R", content: "Para evitar fugas de memoria y mejorar la estabilidad del sistema" },
      { letter: "T", content: "Durante el próximo trimestre" }
    ]
  },
  {
    id: 7,
    title: "Incrementar engagement",
    description: "Incrementar la tasa de engagement en Marruecos del 80% al 95% en 6 meses, mediante la implementación de características localizadas y campañas específicas para este mercado.",
    smart: [
      { letter: "S", content: "Incrementar la tasa de engagement en Marruecos" },
      { letter: "M", content: "Del 80% al 95% (incremento del 15%)" },
      { letter: "A", content: "Mediante implementación de características localizadas y campañas específicas" },
      { letter: "R", content: "Para consolidar nuestra posición en el mercado español" },
      { letter: "T", content: "En 6 meses" }
    ]
  },
  {
    id: 8,
    title: "Expandir alcance mundial",
    description: "Aumentar nuestro alcance mundial del 15% al 25% en los próximos 9 meses, implementando soporte multiidioma y adaptando la plataforma a diferentes husos horarios para diversificar nuestra base de usuarios.",
    smart: [
      { letter: "S", content: "Aumentar nuestro alcance mundial" },
      { letter: "M", content: "Del 15% al 25% (incremento del 10%)" },
      { letter: "A", content: "Implementando soporte multiidioma y adaptación a diferentes husos horarios" },
      { letter: "R", content: "Para diversificar nuestra base de usuarios" },
      { letter: "T", content: "En los próximos 9 meses" }
    ]
  }
]);


</script>


<style scoped>
/* Modern, professional KPI page styles */

ion-content {
  background: linear-gradient(120deg, #f8fafc 0%, #e2e8f0 100%);
  
}

h1 {
  font-size: 1.5rem;
  font-weight: 700;
  color: #1a237e;
  margin-bottom: 18px;
  margin-top: 24px;
  letter-spacing: 0.01em;
}

.ion-padding {
  padding-left: 0 !important;
  padding-right: 0 !important;
}

ion-accordion-group {
  background: none;
  box-shadow: none;
  margin-bottom: 32px;
}

ion-accordion {
  margin-bottom: 14px;
  border-radius: 14px;
  overflow: hidden;
  box-shadow: 0 2px 12px rgba(30, 41, 59, 0.07);
  background: #fff;
  transition: box-shadow 0.2s;
}

ion-accordion.accordion-expanded,
ion-accordion.accordion-expanding {
  box-shadow: 0 4px 24px rgba(59, 130, 246, 0.10);
}

ion-item[slot='header'] {
  --background: #f1f5f9;
  --color: #1e293b;
  font-weight: 600;
  font-size: 1.08rem;
  letter-spacing: 0.01em;
  border-bottom: 1px solid #e2e8f0;
  transition: background 0.2s, color 0.2s;
}

ion-accordion.accordion-expanded ion-item[slot='header'],
ion-accordion.accordion-expanding ion-item[slot='header'] {
  --background: linear-gradient(90deg, #3b82f6 0%, #6366f1 100%);
  --color: #fff;
}

div[slot='content'] {
  background: #fff;
  padding: 18px 22px 12px 22px;
  font-size: 1rem;
  color: #334155;
  border-radius: 0 0 14px 14px;
}

ion-list[inset] {
  background: none;
  margin-top: 10px;
  margin-bottom: 0;
  box-shadow: none;
}

ion-item {
  --background: transparent;
  --color: #475569;
  font-size: 0.98rem;
  border-radius: 8px;
  margin-bottom: 4px;
  padding-left: 0;
  padding-right: 0;
}

ion-label b {
  color: #3b82f6;
  font-weight: 700;
  margin-right: 6px;
}

ion-label {
  font-size: 0.98rem;
  color: #475569;
  letter-spacing: 0.01em;
}

@media (max-width: 600px) {
  h1 {
    font-size: 1.15rem;
    margin-top: 16px;
    margin-bottom: 12px;
  }
  div[slot='content'] {
    padding: 14px 10px 8px 10px;
  }
  ion-accordion {
    margin-bottom: 10px;
  }
}
</style>