<template>
  <div id="app">
    <Timer :deadline="deadline" />
  </div>
</template>

<script>
import Timer from "./components/timer";

export default {
  name: "App",
  components: {
    Timer,
  },
  data() {
    return {
      deadline: Date.parse(new Date()) + 3 * 24 * 60 * 60 * 1000,
    }
  }
};
</script>

<style>
html,
body {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  height: 100%;
  width: 100%;
}
#app {
  width: 100%;
  height: 100%;
}
</style>
