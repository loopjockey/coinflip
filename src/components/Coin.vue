<template lang="pug">
.coin(:class="{ [type]: true, [rarity || 'uncommon']: true, animated: animate }")
  .face.front
    .symbol {{ symbol }}
    .circle
  .face.back
    .symbol {{ symbol }}
    .circle
  figure.side(v-for="n in 20", :key="n")
</template>
    
<script lang="ts">
export default {
  props: {
    type: {
      type: String,
      required: true,
      validator: (value: string) =>
        ["pound", "dollar", "euro", "yen", "bitcoin"].includes(value),
    },
    rarity: {
      type: String,
      required: false,
      default: null,
      validator: (value: string | null) => 
        !value || ['uncommon', 'common', 'rare', 'epic', 'legendary'].includes(value)
    },
    animate: {
      type: Boolean,
      default: false,
    },
  },
  computed: {
    symbol() {
      const symbols: { [type: string]: string } = {
        pound: "£",
        dollar: "$",
        euro: "€",
        yen: "¥",
        bitcoin: "B",
      };
      const type: string = this.type;
      return symbols[type] || "?";
    },
  },
};
</script>
    
    <style scoped lang="scss">
.euro {
  --face: #0066ff;
  --sign: #c49a44;
  --circle: #c49a44;
  --side: #dbb765;
  --slide: #c49a44;
}

.dollar {
  --face: #c0c9d2;
  --sign: #e9eff5;
  --circle: #d6dee4;
  --side: #a0aab4;
  --slide: #c0c9d2;
}

.yen {
  --face: #fcd6bd;
  --sign: #df9061;
  --circle: #f0ae84;
  --side: #f4b389;
  --slide: #df9061;
}

.bitcoin {
  --face: #ffd9b3;
  --sign: #c49a44;
  --circle: #c49a44;
  --side: #dbb765;
  --slide: #c49a44;
}

.pound {
  --face: #ccffcc;
  --sign: #009933;
  --circle: #669999;
  --side: #99ff99;
  --slide: #669999;
}

.dollar.animated {
  animation: spinA 20s -1s linear infinite;
}

.euro.animated {
  animation: spinB 18s -3s linear infinite;
}

.yen.animated {
  animation: spinC 22s -5s linear infinite;
}

.bitcoin.animated {
  animation: spinC 24s -7s linear infinite;
}

.pound.animated {
  animation: spinC 24s -7s linear infinite;
}

.coin {
  width: 100%;
  height: 100%;
  position: absolute;
  transform-style: preserve-3d;
  border-radius: 50%;
  transition: box-shadow 0.3s ease-in-out;
}

.coin:not(.animated):hover {
  box-shadow: 0 0 2em var(--rarity-shadow);
  animation: jiggle 0.3s ease-in-out infinite alternate;
}

@keyframes jiggle {
  0% {
    transform: rotateZ(-5deg);
  }
  100% {
    transform: rotateZ(5deg);
  }
}


// Define rarity-based shadows
.common {
  --rarity-shadow: rgba(200, 200, 200);
}

.uncommon {
  --rarity-shadow: rgb(0, 218, 0);
}

.rare {
  --rarity-shadow: rgba(50, 150, 255);
}

.epic {
  --rarity-shadow: rgba(150, 50, 255);
}

.legendary {
  --rarity-shadow: rgba(255, 215, 0);
}

// Animated coins get a glowing effect based on rarity
.coin.animated {
  box-shadow: 0
}

// Soft glowing animation for animated coins
@keyframes glowEffect {
  0% {
    box-shadow: 0 0 1em var(--rarity-shadow), 0 0 2em var(--rarity-shadow);
  }
  100% {
    box-shadow: 0 0 2em var(--rarity-shadow), 0 0 4em var(--rarity-shadow);
  }
}

.face {
  border-radius: 50%;
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  background: var(--face);
}

.front {
  transform: translate3d(0, 0, 0.4em) rotateY(0);
}

.back {
  transform: rotateY(180deg) translate3d(0, 0, 0.4em);
}

.symbol {
  position: absolute;
  color: var(--sign);
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  font-size: 5em;
}

.circle {
  border-radius: 50%;
  border: 0.35em dashed var(--circle);
  width: 8em;
  height: 8em;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}

.side {
  width: 0.8em;
  height: 1.61em;
  background: var(--side);
  position: absolute;
  left: 50%;
  top: 50%;
  backface-visibility: hidden;
  border-top: 0.2em solid var(--slide);
  border-bottom: 0.2em solid var(--slide);
}

@for $i from 1 through 20 {
  figure:nth-of-type(#{$i}) {
    transform: translate3d(-50%, -50%, 0)
      rotateY(90deg)
      rotateX(($i - 1) * 18deg)
      translateZ(4.9em);
  }
}

@keyframes spinA {
  0% {
    transform: rotateX(0deg) rotateY(0deg) rotateZ(0deg);
  }
  50% {
    transform: rotateX(45deg) rotateY(5 * 360deg) rotateZ(180deg);
  }
  100% {
    transform: rotateX(0deg) rotateY(10 * 360deg) rotateZ(0deg);
  }
}

@keyframes spinB {
  0% {
    transform: rotateX(0deg) rotateY(0deg) rotateZ(0deg);
  }
  50% {
    transform: rotateX(5 * 360deg) rotateY(45deg) rotateZ(25deg);
  }
  100% {
    transform: rotateX(10 * 360deg) rotateY(0deg) rotateZ(0deg);
  }
}

@keyframes spinC {
  0% {
    transform: rotateX(0deg) rotateY(0deg) rotateZ(0deg);
  }
  50% {
    transform: rotateX(45deg) rotateY(-5 * 360deg) rotateZ(180deg);
  }
  100% {
    transform: rotateX(0deg) rotateY(-10 * 360deg) rotateZ(0deg);
  }
}
</style>
    