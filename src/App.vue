<template>
  <div id="app">
    <LiveCard msg="Welcome to Your Vue.js App" />
  </div>
</template>

<script>
import LiveCard from './components/live2d'

export default {
  name: 'App',
  components: {
    LiveCard
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
