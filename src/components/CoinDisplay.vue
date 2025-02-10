<template lang="pug">
.wrapper
  .container(
    v-for="(coinType, i) in ['pound', 'dollar', 'euro', 'yen', 'bitcoin', 'dollar']",
    :key="coinType",
    :class="{ raised: animatedCoins[i] }"
  )
    Coin(
      :type="coinType",
      :animate="animatedCoins[i]",
      :raised="animatedCoins[i]",
      @click="toggleAnimation(i)"
    )
</template>
        
<script lang="ts">
import { ref } from "vue";
import Coin from "./Coin.vue";

export default {
  components: { Coin },
  setup() {
    const animatedCoins = ref<boolean[]>([false, false, false, false, false]);

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
.wrapper {
  width: 100%;
  height: 100%;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
}

.container {
  width: 10em;
  height: 10em;
  position: relative;
  perspective: 300px;

  margin: 2em;
  transform: translateY(0);
  transition: transform 0.3s ease-in-out;
  cursor: pointer;
}

.container.raised {
  box-shadow: 0 11em 3em -2em rgba(0, 0, 0, 0.2);
  transform: translateY(-8em) scale(1.2);
}
</style>
    