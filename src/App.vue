<template>
  <div id="app">
    <Ticker />
  </div>
</template>

<script>
import Ticker from './components/Ticker.vue';

export default {
  name: 'App',
  components: {
    Ticker
  }
}
</script>

<style>
body {
  padding: 0;
  margin: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
