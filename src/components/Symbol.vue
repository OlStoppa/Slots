<template>
  <div class="ymbol">
    <img :src="getImage()" :class="{active: active}" ref="image" />
  </div>
</template>
<script>
import anime from "animejs/lib/anime.es.js";
import { images } from "../images";
export default {
  props: {
    symbol: {
      type: String
    },
    active: {
      type: Boolean
    },
    animate: {
      type: Boolean,
      default: false
    }
  },
  watch: {
    animate(val) {
      if (val) {
        const image = this.$refs.image;
        const xMax = 16;
        anime({
          targets: image,
          easing: "easeInOutSine",
          scale: [{ value: 1.2 }, { value: 1 }],
          duration: 550,
          delay: 200,
          translateX: [
            {
              value: xMax * -1
            },
            {
              value: xMax
            },
            {
              value: xMax / -2
            },
            {
              value: xMax / 2
            },
            {
              value: 0
            }
          ],
          complete: () => {
            this.$emit("done-animate");
            console.log("emititing");
          }
        });
      }
    }
  },
  methods: {
    getImage() {
      switch (this.symbol) {
        case "7":
          return images.seven;
          break;
        case "s":
          return images.sporty;
          break;
        case "l":
          return images.lemon;
          break;
        case "g":
          return images.aubergine;
          break;
        case "y":
          return images.strawberry;
          break;
        case "c":
          return images.cherry;
          break;
        case "b":
          return images.bar;
          break;
        case "a":
          return images.apple;
          break;
      }
    }
  }
};
</script>
<style lang="scss" scoped>
.ymbol {
  background: white;
  height: 100%;
  width: 100%;
  padding: 20px;
  display: flex;

  img {
    width: 100%;
    opacity: 0.4;
    &.active {
      opacity: 1;
    }
  }
}
</style>