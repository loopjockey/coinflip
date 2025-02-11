<template lang="pug">
.scroll-container
  .wrapper
    .container(
      v-for="(coinType, i) in ['pound', 'dollar', 'euro', 'yen', 'bitcoin', 'dollar', 'pound', 'euro', 'yen', 'bitcoin']",
      :key="i",
      ref="coinRefs",
      :class="{ raised: animatedCoins[i], flipping: flippingCoins[i], [['common', 'uncommon', 'rare', 'epic', 'legendary'][i]]: true }",
      :style="{ animationDelay: `${i * 150}ms` }",
      @click="toggleAnimation(i)"
    )
      Coin(
        :type="coinType",
        :rarity="['common', 'uncommon', 'rare', 'epic', 'legendary'][i]",
        :animate="animatedCoins[i]",
        :raised="animatedCoins[i]"
      )

      transition(name="fade")
        .label(v-if="animatedCoins[i]") ★★★
</template>
  
  <script lang="ts">
import { ref, onMounted } from "vue";

export default {
  setup() {
    const animatedCoins = ref<boolean[]>(new Array(10).fill(false));
    const flippingCoins = ref<boolean[]>(new Array(10).fill(true)); // New flipping state
    const coinRefs = ref<(HTMLElement | null)[]>([]);

    const toggleAnimation = (i: number) => {
      animatedCoins.value[i] = !animatedCoins.value[i];
      moveToCenter(i);
    };

    const moveToCenter = (index: number) => {
      const coinElement = coinRefs.value[index];
      if (!coinElement) return;

      const container = document.querySelector(".scroll-container");
      if (!container) return;

      const containerWidth = container.clientWidth;
      const coinRect = coinElement.getBoundingClientRect();
      const containerRect = container.getBoundingClientRect();

      const scrollLeft =
        container.scrollLeft +
        coinRect.left -
        containerRect.left -
        containerWidth / 2 +
        coinRect.width / 2;

      container.scrollTo({ left: scrollLeft, behavior: "smooth" });
    };

    onMounted(() => {
      coinRefs.value = Array.from(document.querySelectorAll(".container"));

      // Staggered removal of flipping effect
      flippingCoins.value.forEach((_, i) => {
        setTimeout(() => {
          flippingCoins.value[i] = false; // Remove flipping one by one
        }, i * 150 + 1000); // Staggered: delay + animation duration (1s)
      });
    });

    return {
      animatedCoins,
      flippingCoins,
      toggleAnimation,
      coinRefs,
    };
  },
};
</script>
  
  <style scoped lang="scss">
.scroll-container {
  padding-left: calc(50vw - 8em);
  padding-right: calc(50vw - 8em);
  width: 100%;
  height: 100vh; /* Ensures it takes full viewport height */
  overflow-x: auto;
  white-space: nowrap;
  display: flex;
  align-items: center; /* Centers coins vertically */
}

.wrapper {
  display: flex;
  flex-wrap: nowrap;
  padding: 1em;
  align-items: center; /* Aligns items in the center */
  gap: 2em;
}

.container {
  width: 10em;
  height: 10em;
  position: relative;
  perspective: 300px;
  margin: 1em;
  transform: translateY(0);
  transition: transform 0.3s ease-in-out;
  cursor: pointer;
  flex: 0 0 auto; /* Prevents shrinking */
}

.container.raised {
  transform: translateY(-8em) scale(1.2);
}

// Define rarity-based shadow colors
.common {
  --rarity-shadow: rgba(200, 200, 200, 0.6);
}

.uncommon {
  --rarity-shadow: rgba(100, 255, 100, 0.6);
}

.rare {
  --rarity-shadow: rgba(50, 150, 255, 0.7);
}

.epic {
  --rarity-shadow: rgba(150, 50, 255, 0.8);
}

.legendary {
  --rarity-shadow: rgba(255, 215, 0, 0.9);
}

// Default container shadow
.container::after {
  content: "";
  position: absolute;
  width: 8em;
  height: 2em;
  background: rgba(0, 0, 0, 0.2);
  border-radius: 50%;
  bottom: -4em;
  left: 50%;
  transform: translateX(-50%) scale(1);
  filter: blur(10px);
  opacity: 0;
  transition: opacity 0.3s ease, transform 0.3s ease-in-out;
}

// Apply glow effect when raised
.container.raised::after {
  opacity: 1;
  transform: translateX(-50%) scale(1.3);
  background: var(--rarity-shadow);
  box-shadow: 0 0 0.1em var(--rarity-shadow), 0 0 0.2em var(--rarity-shadow);
  animation: glowEffect 1.5s infinite alternate;
}

// Soft glowing animation
@keyframes glowEffect {
  0% {
    box-shadow: 0 0 0.1em var(--rarity-shadow), 0 0 0.3em var(--rarity-shadow);
  }
  100% {
    box-shadow: 0 0 0.2em var(--rarity-shadow), 0 0 0.5em var(--rarity-shadow);
  }
}

.label {
  position: absolute;
  bottom: -8em;
  left: 50%;
  transform: translateX(-50%);
  font-size: 1.2em;
  color: #444;
  background: rgba(255, 255, 255, 0.8);
  padding: 0.5em 1em;
  border-radius: 10px;
  box-shadow: 0 0.2em 0.5em rgba(0, 0, 0, 0.1);
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}

.fade-enter,
.fade-leave-to {
  opacity: 0;
}

// Initial flip animation on mount
.container.flipping {
  animation: flipIn 1s ease-out both;
  animation-delay: inherit; // Ensures staggered delays apply
  opacity: 0; // Start invisible
}

// Flip in animation with smooth opacity transition
@keyframes flipIn {
  0% {
    transform: rotateX(90deg) translateY(-50vh) scale(0);
    opacity: 0;
  }
  10% {
    opacity: 0.5;
  }
  50% {
    transform: rotateX(-10deg) translateY(10px) scale(1.1);
    opacity: 1; // Fade in mid-animation
  }
  100% {
    transform: rotateX(0deg) translateY(0) scale(1);
    opacity: 1; // Fully visible when animation is done
  }
}
</style>
  