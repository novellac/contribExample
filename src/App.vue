<template>
  <div id="app"><h1>Table Sort + Filter Example</h1></div>
</template>

<script>
import contributions from "./data/contributions.json";

export default {
  contributions: contributions,
  name: "App"
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
</style>
