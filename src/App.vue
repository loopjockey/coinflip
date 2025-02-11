<template>
  <v-app>
    <CoinDisplay />
  </v-app>
</template>

<script lang="ts" setup>
import { ref, onMounted, onUnmounted } from "vue";
import FallingCoins from "@/components/FallingCoins.vue";
import CoinDisplay from "@/components/CoinDisplay.vue";

const components = [FallingCoins, CoinDisplay];
const currentIndex = ref(0);
const currentComponent = ref(components[currentIndex.value]);
let intervalId: any = null;

const swapComponent = () => {
  currentIndex.value = (currentIndex.value + 1) % components.length;
  currentComponent.value = components[currentIndex.value];
};

onMounted(() => {
  intervalId = setInterval(swapComponent, 2000); // Swap every 5 seconds
});

onUnmounted(() => {
  if (intervalId) clearInterval(intervalId);
});
</script>

<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>
