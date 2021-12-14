<template>
  <StatCalculator />
</template>

<script>
import StatCalculator from "./components/StatCalculator.vue";

export default {
  name: "App",
  components: {
    StatCalculator,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

input {
  padding: 0;
  margin: 0;
}
</style>
