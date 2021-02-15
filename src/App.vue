<template>
  <div id="app">
    <h1>OptVue</h1>
    <br/>
    <chain-selector @create:chart="createChart"/>
    <br/>
    <Plotly id='volumePlot' :key="componentKey" :data="data" :layout="layout" :display-mode-bar="false"></Plotly>
  </div>
</template>

<script>
import ChainSelector from './components/ChainSelector.vue'
import { Plotly } from 'vue-plotly'

export default {
  name: 'App',
  components: {
    ChainSelector,
    Plotly
  },
  data() {
    return {
      componentKey: 0,
      data: [
          {type: 'bar', x: [], y: [], marker: {color: 'red'}, name: 'Put'},
          {type: 'bar', x: [], y: [], marker: {color: 'green'}, name: 'Call'}
      ],
      layout: {
        width: 1500,
        height: 800,
        xaxis: {title: "Volume"},
        yaxis: {title: "Strikes"},
      }
    }
  },
  methods: {
    createChart(strikes, callVolume, putVolume) {
      this.data[0] = {type: 'bar', x: strikes, y: putVolume, marker: {color: 'red'}, name: 'Put'};
      this.data[1] = {type: 'bar', x: strikes, y: callVolume, marker: {color: 'green'}, name: 'Call'};
      this.componentKey += 1;
    }
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
