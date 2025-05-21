<template>
  <ion-app>
    <ion-split-pane content-id="main-content">
      <NavSidebar />
      <ion-router-outlet id="main-content"></ion-router-outlet>
    </ion-split-pane>
  </ion-app>
</template>

<script setup lang="ts">
import { IonApp, IonRouterOutlet, IonSplitPane } from '@ionic/vue';
import NavSidebar from '@/components/NavSidebar.vue';
</script>

<style scoped>
ion-split-pane {
  --side-max-width: 280px;
}
</style>