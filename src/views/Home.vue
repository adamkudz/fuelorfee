<template>
  <div class="HOMECONTAINER">
    <h1 class="text-center">Fuel or Fee?</h1>
    <div class="INPUTCONTAINER">
      <div class="inputBox pa-3">
        <h2 class="pt-2">Home Fuel Price</h2>
        <div>
          <span class="dollarSign">$</span>
          <input step="any" v-model.number="homeFuel" type="number" />
        </div>
      </div>
      <div class="inputBox pa-3">
        <h2 class="pt-2">FBO Fee</h2>
        <div>
          <span class="dollarSign">$</span>
          <input step="any" v-model.number="fboFee" type="number" />
        </div>
      </div>
      <div class="inputBox pa-3">
        <h2 class="pt-2">FBO Fuel Price</h2>
        <div>
          <span class="dollarSign">$</span>
          <input step="any" v-model.number="fboFuel" type="number" />
        </div>
      </div>
      <div class="inputBox pa-3">
        <h2 class="pt-2">Fuel Required</h2>
        <div>
          <input step="any" v-model.number="fboGallons" type="number" />
        </div>
      </div>
    </div>
    <div class="BUTTONCONTAINER">
      <div class="buttonBox d-flex justify-center">
        <v-btn @click="calculateResult">Calculate</v-btn>
      </div>
    </div>
    <div class="RESULTSCONTAINER">
      <div v-show="getFuel">
        <h2>Get Fuel</h2>
        <p>Cost of getting fuel is ${{ this.fboTotal }}</p>
        <p>Cost of paying the Fee plus fuel at home is ${{ this.homeTotal }}</p>
        <p>Buy fuel here and save ${{ this.homeTotal - this.fboTotal }}</p>
      </div>
      <div v-show="payFee">
        <h2>Pay the Fee</h2>
        <p>Cost of getting fuel here is ${{ this.fboTotal }}</p>
        <p>
          Cost of paying the Fee (${{ this.fboFee }}) plus
          {{ this.fboGallons }} gallons at home is ${{ this.homeTotal }}
        </p>
        <p>Pay the fee and save ${{ this.fboTotal - this.homeTotal }}</p>
      </div>
    </div>
    <div class="BUTTONCONTAINER">
      <div class="buttonBox d-flex justify-center">
        <v-btn @click="reset">RESET</v-btn>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src

export default {
  name: "Home",
  components: {},
  data() {
    return {
      homeFuel: null,
      fboFee: null,
      fboFuel: null,
      fboGallons: null,
      getFuel: false,
      payFee: false,
    };
  },
  methods: {
    calculateResult: function() {
      this.payFee = false;
      this.getFuel = false;
      if (this.homeTotal < this.fboTotal) {
        this.payFee = true;
      } else {
        this.getFuel = true;
      }
    },
    reset: function() {
      this.homeFuel = null;
      this.fboFee = null;
      this.fboFuel = null;
      this.fboGallons = null;
      this.getFuel = false;
      this.payFee = false;
    },
  },
  computed: {
    homeTotal() {
      return (this.homeFuel * this.fboGallons + this.fboFee).toFixed(2);
    },
    fboTotal() {
      return (this.fboFuel * this.fboGallons).toFixed(2);
    },
  },
};
</script>
<style lang="scss" scoped>
.INPUTCONTAINER {
  background: lightblue;
  max-width: 400px;
  margin: auto;
}
.inputBox {
  display: flex;
  justify-content: space-between;
  gap: 1em;
  margin: auto;
}
input {
  font-size: 30px;
  max-width: 100px;
}
.dollarSign {
  font-size: 2em;
}
</style>
