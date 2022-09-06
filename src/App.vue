<template>
  <div id="app">
    <jackpot />
  </div>
</template>

<script>
import jackpot from './components/jackpot.vue'

export default {
  name: 'App',
  components: {
    jackpot
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  width: 100%;
  height: 100vh;
  padding: 0;
  margin: 0;
  color: #60208b;
}
</style>
