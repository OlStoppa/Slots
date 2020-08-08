<template>
  <div class="reel-set">
    <Reel v-for="i in 5" :key="i" :order="i" @results="handleResults" />
  </div>
</template>
<script>
import EventBus from "../event-bus";
import Reel from "./Reel.vue";
import { WIN_LINES } from "../util/constants";
export default {
  components: {
    Reel
  },
  data() {
    return {
      resultsArr: [],
      reelsSpinning: null,
      winningLines: []
    };
  },
  methods: {
    handleResults(results) {
      this.reelsSpinning--;
      this.resultsArr[results.order - 1] = results.results;
      if (this.reelsSpinning === 0) {
        this.evaluateWin();
      }
    },
    evaluateWin() {
      this.winningLines = [];
      for (let lineIdx = 0; lineIdx < WIN_LINES.length; lineIdx++) {
        let streak = 0;
        let currentKind = null;
        for (let cordIdx = 0; cordIdx < WIN_LINES[lineIdx].length; cordIdx++) {
          let cords = WIN_LINES[lineIdx][cordIdx];
          let symbolAtCords = this.resultsArr[cords[0]][cords[1]];

          if (cordIdx === 0) {
            currentKind = symbolAtCords;
            streak += 1;
          } else {
            if (symbolAtCords !== currentKind) {
              break;
            }
            streak += 1;
          }
        }
        if (streak >= 3) {
          this.winningLines.push(lineIdx);
        }
      }
      this.highlightWins(0);
    },
    highlightWins(currentIndex) {
      if (!this.winningLines) {
        return;
      }
      if (currentIndex > this.winningLines.length - 1) {
        return;
      }
      EventBus.$emit("HIGHLIGHT", this.winningLines[currentIndex]);
      setTimeout(() => {
        this.highlightWins(currentIndex + 1);
      }, 800);
    }
  },
  mounted() {
    EventBus.$on("SPIN", () => {
      this.reelsSpinning = 5;
    });
  }
};
</script>
<style scoped>
.reel-set {
  flex: 1;
  background: white;
  display: flex;
}
</style>