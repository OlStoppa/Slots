<template>
  <div id="app">
    <HelloWorld />
  </div>
</template>
<script>
import HelloWorld from "./components/HelloWorld.vue";

export default {
  name: "App",
  components: {
    HelloWorld
  },
  mounted() {
    // document.documentElement.requestFullscreen();
    // screen.orientation.lock("landscape");
  }
};
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: none;
}
</style>
