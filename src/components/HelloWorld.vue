<template>
  <div class="hello">
    <!-- <div class="play-container">
      <ReelSet />
    </div>
    <div class="btn-container">
      <button @click="handleSpin">Spin</button>
    </div>-->
    <div class="top-bar"></div>
    <div class="main">
      <div class="play-container">
        <ReelSet />
      </div>
    </div>
    <div class="bottom-bar">
      <button @click="handleSpin">Spin</button>
    </div>
  </div>
</template>

<script>
import ReelSet from "./ReelSet.vue";
import EventBus from "../event-bus";
export default {
  name: "HelloWorld",
  props: {
    msg: String
  },
  components: {
    ReelSet
  },
  methods: {
    handleSpin() {
      EventBus.$emit("SPIN");
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.hello {
  display: flex;
  flex-direction: column;
  background-image: url("~@/assets/free_games_back.jpg");
  background-size: cover;
}
.play-container {
  height: calc(100vh - 60px);
  width: 100vw;
  background-color: orange;
  display: flex;
  overflow: hidden;
}
.btn-container {
  height: 60px;
  width: 100vw;
  background-color: transparent;
}
.bottom-bar {
  width: 100%;
  background-image: url("~@/assets/bottom_back.png");
  background-size: contain;
  background-repeat: no-repeat;
  height: 70px;
}
</style>
