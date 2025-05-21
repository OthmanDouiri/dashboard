<template>
  <ion-header :translucent="true">
    <ion-toolbar>
      <!-- Removed sidebar toggle button -->
      <ion-title>{{ title }}</ion-title>
    </ion-toolbar>
  </ion-header>

  <ion-header collapse="condense">
    <ion-toolbar>
      <ion-title size="large">{{ title }}</ion-title>
    </ion-toolbar>
  </ion-header>
</template>

<script setup lang="ts">
import { IonHeader, IonTitle, IonToolbar } from '@ionic/vue';

defineProps<{
  title: string;
}>();
</script>

<style scoped>
/* Add any custom header styling here */
ion-toolbar {
  --background: linear-gradient(120deg, #ffffff, #f8fafc);
  box-shadow: 0 2px 10px rgba(0,0,0,0.03);
}

ion-title {
  font-weight: 600;
  color: #1a1f2b;
}
</style>
