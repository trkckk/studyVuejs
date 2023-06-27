<template>
  <MasterMind/>
</template>

<script>
import MasterMind from './components/MasterMind.vue'

export default {
  name: 'App',
  components: {
    MasterMind
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
