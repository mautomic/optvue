<template>
  <div id="app">
    <h1>OptVue</h1>
    <br/>
    <chain-selector/>
  </div>
</template>

<script>
import ChainSelector from './components/ChainSelector.vue'

export default {
  name: 'App',
  components: {
    ChainSelector,
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
