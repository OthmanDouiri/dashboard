<template>
  <ion-menu content-id="main-content" type="overlay" menu-id="main-menu">
    <ion-content>
      <div class="sidebar-header">
        <div class="logo-container">
          <div class="logo">D</div>
          <div class="logo-text">
            <ion-list-header>Dashboard</ion-list-header>
            <ion-note>Data Visualization</ion-note>
          </div>
        </div>
      </div>

      <div class="menu-container">
        <ion-menu-toggle :auto-hide="false" v-for="(p, i) in appPages" :key="i">
          <ion-item
            @click="selectedIndex = i"
            router-direction="root"
            :router-link="p.url"
            lines="none"
            :detail="false"
            class="menu-item"
            :class="{ selected: selectedIndex === i }"
          >
            <ion-icon aria-hidden="true" slot="start" :ios="p.iosIcon" :md="p.mdIcon"></ion-icon>
            <ion-label>{{ p.title }}</ion-label>
          </ion-item>
        </ion-menu-toggle>
      </div>

      <div class="sidebar-footer">
        <div class="version">v1.0.0</div>
      </div>
    </ion-content>
  </ion-menu>
</template>

<script setup lang="ts">
import { IonContent, IonIcon, IonItem, IonLabel, IonListHeader, IonMenu, IonMenuToggle, IonNote } from '@ionic/vue';
import { rocketOutline, rocketSharp, pulseOutline, pulseSharp, speedometerOutline, speedometerSharp } from 'ionicons/icons';
import { ref, onMounted, watch } from 'vue';
import { useRoute } from 'vue-router';

const selectedIndex = ref(0);
const appPages = [
  {
    title: 'Negocio',
    url: '/negocio',
    iosIcon: rocketOutline,
    mdIcon: rocketSharp,
  },
  {
    title: 'Técnico',
    url: '/tecnico',
    iosIcon: pulseOutline,
    mdIcon: pulseSharp,
  },
  {
    title: 'KPIs',
    url: '/kpis',
    iosIcon: speedometerOutline,
    mdIcon: speedometerSharp,
  },
];

const route = useRoute();

// 🔄 Function to update selectedIndex based on current URL
const updateSelectedIndex = () => {
  const currentPath = route.path;
  const index = appPages.findIndex(page => page.url === currentPath);
  if (index !== -1) {
    selectedIndex.value = index;
  }
};

// Execute when app loads
onMounted(updateSelectedIndex);

// Execute when route changes
watch(route, updateSelectedIndex);
</script>

<style scoped>
/* Base Menu Container */
ion-menu {
  --ion-background-color: #1a1f2b;
  --ion-text-color: #e2e8f0;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.2);
}

ion-menu ion-content {
  --background: linear-gradient(135deg, #1a1f2b 0%, #2d3748 100%);
  --padding-top: 0;
  --padding-bottom: 0;
  --padding-start: 0;
  --padding-end: 0;
}

/* Sidebar Header */
.sidebar-header {
  padding: 24px 16px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.07);
}

.logo-container {
  display: flex;
  align-items: center;
}

.logo {
  width: 42px;
  height: 42px;
  background: linear-gradient(135deg, #3b82f6 0%, #8b5cf6 100%);
  border-radius: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 24px;
  font-weight: 700;
  color: white;
  margin-right: 12px;
  box-shadow: 0 4px 12px rgba(59, 130, 246, 0.3);
}

.logo-text ion-list-header {
  font-size: 20px;
  font-weight: 700;
  color: #ffffff;
  padding: 0;
  margin: 0 0 4px;
}

.logo-text ion-note {
  font-size: 12px;
  color: #a0aec0;
  padding: 0;
  margin: 0;
}

/* Menu Container */
.menu-container {
  padding: 24px 12px;
}

/* Menu Items */
ion-menu-toggle {
  margin-bottom: 8px;
}

.menu-item {
  --background: transparent;
  --background-hover: rgba(255, 255, 255, 0.06);
  --background-activated: rgba(255, 255, 255, 0.08);
  --border-radius: 10px;
  --padding-start: 16px;
  --padding-end: 16px;
  --min-height: 48px;
  font-weight: 500;
  margin: 0;
  transition: all 0.2s ease;
}

.menu-item:hover {
  --background: rgba(255, 255, 255, 0.05);
  transform: translateX(3px);
}

.menu-item ion-label {
  font-size: 15px;
  color: #cbd5e0;
  margin-left: 4px;
}

.menu-item ion-icon {
  font-size: 18px;
  color: #718096;
}

/* Selected item */
.menu-item.selected {
  --background: rgba(59, 130, 246, 0.15);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  border-left: 3px solid #3b82f6;
}

.menu-item.selected ion-label {
  color: #3b82f6;
  font-weight: 600;
}

.menu-item.selected ion-icon {
  color: #3b82f6;
}

/* Footer */
.sidebar-footer {
  padding: 16px;
  border-top: 1px solid rgba(255, 255, 255, 0.07);
  position: absolute;
  bottom: 0;
  width: 100%;
  text-align: center;
}

.version {
  font-size: 12px;
  color: #718096;
}
</style>
