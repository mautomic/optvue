<template>
  <div id="chain-selector" class="medium-container">

    <form @submit.prevent="getChains()">
      <input type="text" v-model="symbol" placeholder="SPY" maxlength="6" />
      <label></label>
      <button>Get Chains</button>
      <label></label>
    </form>

    <form @submit.prevent="chart()">
      <select id="expiry-selector"></select>
      <label></label>
      <button>Chart Options</button>
    </form>

    <small_form>
      <select id="strike-count-selector" v-model="strikeCount">
        <option>Strike Count</option>
        <option>10</option>
        <option>30</option>
        <option>50</option>
        <option>100</option>
      </select>
      <label></label>
    </small_form>
  </div>
</template>

<script>
var CONFIG = require('../config.json');

export default {
  name: 'ChainSelector',
  data() {
    return {
      symbol: '',
      strikeCount: 'Strike Count',
    }
  },
  methods: {

    getChains() {
      var symbol = this.symbol.toUpperCase();
      let tdUrl = `https://api.tdameritrade.com/v1/marketdata/chains?apikey=${CONFIG.apiKey}&symbol=${symbol}&strikeCount=1`

      var selectElement = document.getElementById('expiry-selector');
      var i, L = selectElement.options.length - 1;
      for(i = L; i >= 0; i--) {
        selectElement.remove(i);
      }

      fetch(tdUrl)
          .then(res => res.json())
          .then((data) => {
            let expirationDates = []
            const callMap = data.callExpDateMap;
            for (let expDate in callMap) {
              const date = expDate.split(":")[0]
              expirationDates.push(date)
            }

            const selectOptions = document.getElementById("expiry-selector").options;
            for (let index = 0; index < expirationDates.length; index++) {
              const option = document.createElement("option");
              option.value = expirationDates[index];
              option.innerHTML = expirationDates[index];
              selectOptions.add(option);
            }
          })
          .catch(console.log)
    },

    chart() {
      var symbol = this.symbol.toUpperCase();
      var expirySelector = document.getElementById("expiry-selector");
      var chainCountSelector = document.getElementById("strike-count-selector");
      let strikeCount = 30;
      if(chainCountSelector.value !== 'Strike Count') {
        strikeCount = chainCountSelector.value;
      }
      let tdUrl = `https://api.tdameritrade.com/v1/marketdata/chains?apikey=${CONFIG.apiKey}
&symbol=${symbol}&strikeCount=${strikeCount}&toDate=${expirySelector.value}&fromDate=${expirySelector.value}`;

      let putVolumes = [];
      let callVolumes = [];
      let putOIs = [];
      let callOIs = [];
      let strikes = [];
      let callGammaExposures = new Map();
      let putGammaExposures = new Map();
      let SPY = 427.92;

      fetch(tdUrl)
          .then(res => res.json())
          .then((data) => {

            const callMap = data.callExpDateMap;
            const putMap = data.putExpDateMap;

            for (let expDate in callMap) {
              let callStrikeMap = callMap[expDate];
              for (let aStrike in callStrikeMap) {
                let call = callStrikeMap[aStrike][0];
                callVolumes.push(call.totalVolume);
                callOIs.push(call.openInterest);
                strikes.push(call.strikePrice);
                let callGamma = call.openInterest * call.gamma * 100 * SPY * SPY * .01;
                callGammaExposures.set(call.strikePrice, callGamma);
              }

              let putStrikeMap = putMap[expDate];
              for (let aStrike in putStrikeMap) {
                let put = putStrikeMap[aStrike][0];
                putVolumes.push(put.totalVolume);
                putOIs.push(put.openInterest);
                let putGamma = put.openInterest * put.gamma * 100 * SPY * SPY * -.01;
                putGammaExposures.set(put.strikePrice, putGamma);
              }
            }

            let gammaExposures = [];
            for (let strike of strikes) {
              let callGamma = 0;
              if (callGammaExposures.has(strike)) {
                callGamma = callGammaExposures.get(strike);
              } else {
                continue;
              }

              let putGamma = 0;
              if (putGammaExposures.has(strike)) {
                putGamma = putGammaExposures.get(strike);
              } else {
                continue;
              }
              let gammaExposure = callGamma + putGamma;
              console.log(strike);
              console.log(gammaExposure);
              gammaExposures.push(gammaExposure);
            }

            this.$emit('create:chart', strikes, callVolumes, putVolumes, callOIs, putOIs, gammaExposures);
          })
          .catch(console.log)
    }
  }
}
</script>

<style scoped>

.medium-container {
  display: flex;
  justify-content: center;
}

form {
  padding-top: 10px;
  display: flex;
  width: 320px;
  height: 50px;
}

small_form {
  padding-top: 10px;
  display: flex;
  width: 160px;
  height: 50px;
}

label {
  font-size: 17px;
  height: 30px;
  padding-left: 10px;
  padding-right: 10px;
}
input {
  width: 120px;
  height: 50px;
  padding-right: 10px;
}
select {
  width: 130px;
  height: 50px;
}
button {
  height: 50px;
  width: 150px;
  padding-left: 20px;
  padding-right: 20px;
}
</style>
