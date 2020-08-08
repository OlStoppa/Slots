<template>
  <div ref="reel" class="reel" @click="scrollByOffset(10)">
    <div v-for="(symbol, index) in symbolArr" :key="index" class="symbol-container">
      <Character
        :symbol="symbol"
        :active="allActive ? allActive : highlightIndex.includes(index)"
        :animate="highlightIndex.includes(index)"
      />
    </div>
  </div>
</template>
<script>
import { WIN_LINES } from "../util/constants";
import Character from "./Symbol.vue";
import anime from "animejs/lib/anime.es.js";
import EventBus from "../event-bus";
export default {
  components: {
    Character
  },
  props: {
    order: {
      type: Number
    }
  },
  data() {
    return {
      symbols: "agbbcgla777syycl",
      currentTransY: 0,
      allActive: false,
      reelPosition: 0,
      activeSymbols: [],
      results: [],
      highlightIndex: [],
      currentOffset: 0
    };
  },
  computed: {
    symbolArr() {
      return this.symbols.repeat(10).split("");
    }
  },
  methods: {
    startScroll() {
      const reel = this.$refs.reel;
      console.log(this.currentTransY);
      anime({
        targets: reel,
        translateY: 12000,
        direction: "alternate"
      }).finished;
    },
    scrollByOffset() {
      this.highlightIndex = [];
      this.allActive = true;
      let offset = Math.ceil(Math.random() * 10);
      this.currentOffset = offset;
      const reel = this.$refs.reel;
      const symbolHeight = reel.offsetHeight / 3;
      const reelHeight = symbolHeight * this.symbols.length;
      let offsetHeight = symbolHeight * offset;
      const ypos = this.currentTransY + offsetHeight + reelHeight * 3;
      anime({
        targets: reel,
        translateY: ypos,
        duration: 2000,
        delay: this.order * 300,
        easing: "easeInOutQuad",
        update: () => {},
        complete: () => {
          const rollUp =
            ypos -
            reelHeight * 3 -
            symbolHeight * (this.symbols.length - offset) -
            symbolHeight * offset;
          this.reelPosition = this.symbols.length - offset - 3;
          reel.style.transform = `translateY(${rollUp}px)`;
          this.allActive = false;
          this.activeSymbols = [
            this.reelPosition,
            this.reelPosition + 1,
            this.reelPosition + 2
          ];
          this.results = this.activeSymbols.map(
            position => this.symbolArr[position]
          );
          this.$emit("results", { order: this.order, results: this.results });
        }
      });
    }
  },
  mounted() {
    this.$nextTick(() => {
      const { reel } = this.$refs;
      const symbolHeight = reel.offsetHeight / 3;
      const deckHeight = symbolHeight * this.symbols.length;
      this.currentTransY = (deckHeight * 8 - reel.offsetHeight) * -1;

      reel.style.transform = `translateY(${this.currentTransY}px)`;
      const initInactive = this.symbolArr.length - this.symbols.length * 2;
      this.activeSymbols[
        (initInactive - 4, initInactive - 3, initInactive - 2)
      ];
    });
    EventBus.$on("SPIN", () => {
      this.opacityClear = true;
      this.scrollByOffset(10);
    });
    let self = this;
    EventBus.$on("HIGHLIGHT", line => {
      const index = WIN_LINES[line][this.order - 1];
      this.highlightIndex = [
        this.symbolArr.length -
          this.symbols.length -
          this.currentOffset -
          (2 - index[1]) -
          1
      ];
    });
  }
};
</script>
<style lang="scss" scoped>
.reel {
  width: 20%;
  height: 100%;
  background: pink;
}
.symbol-container {
  height: calc(100% / 3);
  width: 100%;
  position: relative;
}
</style>
