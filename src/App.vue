<template>
  <div id="app">
    <the-line></the-line>
  </div>
</template>

<script>
import TheLine from "./components/Line";
import "./assets/styles.css";
export default {
  name: "App",
  components: {
    TheLine
  }
};
</script>

<style lang="scss">
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
