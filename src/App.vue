<template>
  <div id="app">
    <h1>OptVue</h1>
    <br/>
    <chain-selector @create:chart="createChart"/>
    <br/>
    <Plotly id='volumePlot' :key="volume_componentKey" :data="volume_data" :layout="volume_layout" :display-mode-bar="false"></Plotly>
    <Plotly id='openInterestPlot' :key="oi_componentKey" :data="oi_data" :layout="oi_layout" :display-mode-bar="false"></Plotly>
    <Plotly id='gammaPlot' :key="gamma_componentKey" :data="gamma_data" :layout="gamma_layout" :display-mode-bar="false"></Plotly>
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
      volume_componentKey: 0,
      volume_data: [
          {type: 'bar', x: [], y: [], marker: {color: 'red'}, name: 'Put'},
          {type: 'bar', x: [], y: [], marker: {color: 'green'}, name: 'Call'}
      ],
      volume_layout: {width: 1400, height: 800, xaxis: {title: "Strikes"}, yaxis: {title: "Volume"}},
      oi_componentKey: 0,
      oi_data: [
        {type: 'bar', x: [], y: [], marker: {color: 'red'}, name: 'Put'},
        {type: 'bar', x: [], y: [], marker: {color: 'green'}, name: 'Call'}
      ],
      oi_layout: {width: 1400, height: 800, xaxis: {title: "Strikes"}, yaxis: {title: "Open Interest"}},
      gamma_componentKey: 0,
      gamma_data: [
        {type: 'bar', x: [], y: [], marker: {color: 'blue'}, name: 'Put'},
      ],
      gamma_layout: {width: 1400, height: 800, xaxis: {title: "Strikes"}, yaxis: {title: "Gamma"}},
    }
  },
  methods: {
    createChart(strikes, callVolume, putVolume, callOIs, putOIs, gammaExposures) {
      this.volume_data[0] = {type: 'bar', x: strikes, y: putVolume, marker: {color: 'red'}, name: 'Put'};
      this.volume_data[1] = {type: 'bar', x: strikes, y: callVolume, marker: {color: 'green'}, name: 'Call'};
      this.volume_componentKey += 1;
      this.oi_data[0] = {type: 'bar', x: strikes, y: putOIs, marker: {color: 'red'}, name: 'Put'};
      this.oi_data[1] = {type: 'bar', x: strikes, y: callOIs, marker: {color: 'green'}, name: 'Call'};
      this.oi_componentKey += 1;
      this.gamma_data[0] = {type: 'bar', x: strikes, y: gammaExposures, marker: {color: 'blue'}, name: 'Gamma'};
      this.gamma_componentKey += 1;
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
