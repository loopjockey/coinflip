<template>
  <div class="falling-container">
    <Coin
      v-for="coin in fallingCoins"
      :key="coin.id"
      :type="coin.type"
      class="falling-coin"
      :style="{ '--startX': coin.startX, '--fallDuration': coin.duration }"
    />
  </div>
</template>
  
  <script setup lang="ts">
import { ref, onMounted } from "vue";
import Coin from "./Coin.vue"; // Reuse the existing Coin component

const fallingCoins = ref([]);
const coinTypes = ["pound", "dollar", "euro", "yen", "bitcoin"];
const coinCount = 10; // Number of coins falling at any time

const generateFallingCoins = () => {
  fallingCoins.value = []; // Clear existing coins
  for (let i = 0; i < coinCount; i++) {
    spawnCoin(i);
  }
};

// Function to spawn a single coin
const spawnCoin = (id) => {
  fallingCoins.value.push({
    id,
    type: coinTypes[Math.floor(Math.random() * coinTypes.length)], // Random coin type
    startX: `${Math.random() * 90}vw`, // Random X position
    duration: `${1.5 + Math.random() * 1.5}s`, // Random fall speed
  });

  // Remove coin after it falls and spawn a new one
  setTimeout(() => {
    fallingCoins.value = fallingCoins.value.filter((c) => c.id !== id);
    spawnCoin(id); // Respawn a new coin
  }, parseFloat(fallingCoins.value.find((c) => c.id === id)?.duration) * 1000);
};

onMounted(() => {
  generateFallingCoins(); // Start the loop
});
</script>
  
  <style scoped>
.falling-container {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  pointer-events: none;
  overflow: hidden;
}

.falling-coin {
  position: absolute;
  width: 10em;
  height: 10em;
  top: -20vh; /* Start above screen */
  left: var(--startX);
  opacity: 0;
  transform: scale(0.5);
  animation: fallAnimation var(--fallDuration) ease-in-out forwards;
}

@keyframes fallAnimation {
  0% {
    opacity: 0;
    transform: translateY(-20vh) scale(0.5);
  }
  20% {
    opacity: 1;
  }
  100% {
    transform: translateY(100vh) scale(1);
    opacity: 0;
  }
}
</style>
  