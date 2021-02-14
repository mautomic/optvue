<template>
  <div id="chain-selector" class="small-container">
    <form>
      <input type="text" placeholder="SPY" maxlength="6" />

      <label></label>
      <button>Get Chains</button>
      <label></label>

      <select v-model="expiry">
        <option value="Expiry Date" disabled hidden>Expiry Date</option>
      </select>

      <label></label>
      <button>Chart</button>
    </form>
  </div>
</template>

<script>
export default {
  name: 'ChainSelector',
  data() {
    return {
      expiry: "Expiry Date"
    }
  },
}
</script>

<style scoped>
.small-container {
  width: 570px;
}
form {
  padding-top: 10px;
  display: flex;
  width: 550px;
  height: 50px;
}
label {
  font-size: 17px;
  height: 30px;
  padding-left: 10px;
  padding-right: 10px;
}
input {
  width: 100px;
  height: 50px;
  padding-right: 10px;
}
select {
  width: 150px;
  height: 50px;
}
button {
  height: 50px;
  padding-left: 20px;
  padding-right: 20px;
}
</style>