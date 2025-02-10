<template lang="pug">
.scroll-container
  .wrapper
    .container(
      v-for="(coinType, i) in ['pound', 'dollar', 'euro', 'yen', 'bitcoin', 'dollar', 'pound', 'euro', 'yen', 'bitcoin']",
      :key="i",
      :class="{ raised: animatedCoins[i] }",
      @click="toggleAnimation(i)"
    )
      Coin(
        :type="coinType",
        :animate="animatedCoins[i]",
        :raised="animatedCoins[i]"
      )

      transition(name="fade")
        .label(v-if="animatedCoins[i]") ★★★ 
</template>
  
  <script lang="ts">
import { ref } from "vue";

export default {
  setup() {
    const animatedCoins = ref<boolean[]>(new Array(10).fill(false));

    const toggleAnimation = (i: number) => {
      animatedCoins.value[i] = !animatedCoins.value[i];
    };

    return {
      animatedCoins,
      toggleAnimation,
    };
  },
};
</script>
  
  <style scoped lang="scss">
.scroll-container {
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

.container.raised::after {
  opacity: 1;
  transform: translateX(-50%) scale(1.3); /* Slightly bigger for depth effect */
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
</style>
  