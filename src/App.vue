<template>
  <div id="app" class="box">
      <ChartContainer/>
  </div>
</template>

<script>

  import ChartContainer from "./components/ChartContainer";
export default {
  name: 'App',
  components: {
      ChartContainer,

  }
}
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
</style>
