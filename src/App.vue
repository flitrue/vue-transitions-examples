<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <div>
      <div class="container">
        <Transitions/>
        <TransLate/>
      </div>
    </div>
  </div>
</template>

<script>
import Transitions from "./components/Transitions.vue";
import TransLate from "./components/TransLate.vue";

export default {
  name: "app",
  data() {
    return {};
  },
  components: {
    Transitions,
    TransLate
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.container {
  display: inline-block;
  width: 500px;
}
</style>
