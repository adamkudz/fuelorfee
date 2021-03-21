<template>
  <div class="HOMECONTAINER">
    <h1 v-if="!showResults" class="text-center mt-2">Fuel or Fee?</h1>
    <h1
      v-if="showResults && getFuel"
      class="text-center mt-2 mx-2"
      style="background:var(--green);"
    >
      FUEL!
    </h1>
    <h1
      v-if="showResults && payFee"
      class="text-center mt-2 mx-2"
      style="background:var(--red);"
    >
      FEE!
    </h1>
    <div class="INPUTCONTAINER ma-2">
      <div class="verticalInput text-center pa-1">
        <h2 class="">FBO Fee</h2>
        <div>
          <span class="dollarSign">$</span>
          <input
            ref="fboFee"
            autofocus
            step="any"
            v-model.number="fboFee"
            type="number"
            inputmode="numeric"
            :disabled="isDisabled"
          />
        </div>
      </div>
      <div class="FUELCONTAINER pa-1">
        <div class="verticalInput pa-1">
          <h2 class="pt-2">Fuel Req</h2>
          <div>
            <input
              class="text-right"
              step="any"
              v-model.number="fboGallons"
              type="number"
              inputmode="numeric"
              :disabled="isDisabled"
            /><span> gal</span>
          </div>
        </div>
        <div class="verticalInput ">
          <h2 class="pt-2 text-center">Fuel Price</h2>
          <div>
            <span class="dollarSign">$</span>
            <input
              class="text-center"
              step="any"
              v-model.number="fboFuel"
              type="number"
              inputmode="numeric"
              :disabled="isDisabled"
            /><span> per gal</span>
          </div>
        </div>
      </div>
      <div class="verticalInput text-center">
        <h2 class="pt-2">Home Fuel Price</h2>
        <div>
          <span class="dollarSign">$</span>
          <input
            step="any"
            v-model.number="homeFuel"
            type="number"
            inputmode="numeric"
            :disabled="isDisabled"
          /><span> per gal</span>
        </div>
      </div>
    </div>
    <div v-if="showCalculate" class="BUTTONCONTAINER">
      <div class="buttonBox d-flex justify-center mt-5">
        <v-btn @click="calculateResult">Calculate</v-btn>
      </div>
    </div>
    <div v-if="showResults" class="RESULTSCONTAINER text-center pa-2 ">
      <div v-show="getFuel" style="backgroundColor: var(--green);">
        <h2 class="mb-3">Get Fuel</h2>
        <p class="mb-1">
          Cost of getting fuel is <strong>${{ this.fboTotal }}</strong>
        </p>
        <p class="mb-1">
          Cost of paying the Fee plus fuel at home is
          <strong> ${{ this.homeTotal }}</strong>
        </p>
        <p>
          Buy fuel here and save
          <strong>${{ this.feeSavings }}</strong>
        </p>
        <div class="BUTTONCONTAINER">
          <div
            v-if="showReset"
            class="buttonBox d-flex justify-center mt-5 mb-3"
          >
            <v-btn @click="reset">RESET</v-btn>
          </div>
        </div>
      </div>
      <div v-show="payFee" style="backgroundColor: var(--red) ;">
        <h2 class="mb-3">Pay the Fee</h2>
        <p class="mb-1">
          Cost of getting fuel here is <strong>${{ this.fboTotal }}</strong>
        </p>
        <p class="mb-1">
          Cost of paying the Fee <strong>(${{ this.fboFee }})</strong> plus
          {{ this.fboGallons }} gallons at home is
          <strong> ${{ this.homeTotal }}</strong>
        </p>
        <p>
          Pay the fee and save
          <strong>${{ this.feeSavings }}</strong>
        </p>
        <div class="BUTTONCONTAINER">
          <div v-if="showReset" class="buttonBox d-flex justify-center">
            <v-btn @click="reset">RESET</v-btn>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src

export default {
  name: "Home",
  components: {},
  data: () => ({
    homeFuel: null,
    fboFee: null,
    fboFuel: null,
    fboGallons: null,
    getFuel: false,
    payFee: false,
    showReset: false,
    showCalculate: true,
    showResults: false,
    isDisabled: false,
    bgColor: "red",
  }),
  methods: {
    calculateResult: function() {
      this.payFee = false;
      this.getFuel = false;
      this.showReset = true;
      this.showCalculate = false;
      this.showResults = true;
      this.isDisabled = true;

      if (this.homeTotal < this.fboTotal) {
        this.payFee = true;
        this.bgColor = "crimson";
      } else {
        this.getFuel = true;
        this.bgColor = "";
      }
    },
    reset: function() {
      this.homeFuel = null;
      this.fboFee = null;
      this.fboFuel = null;
      this.fboGallons = null;
      this.getFuel = false;
      this.payFee = false;
      this.showReset = false;
      this.showCalculate = true;
      this.showResults = false;
      this.isDisabled = false;
      this.$refs.fboFee.focus();
    },
  },
  computed: {
    homeTotal() {
      return (this.homeFuel * this.fboGallons + this.fboFee).toFixed(2);
    },
    fboTotal() {
      return (this.fboFuel * this.fboGallons).toFixed(2);
    },
    feeSavings() {
      if (this.homeTotal > this.fboTotal) {
        return (this.homeTotal - this.fboTotal).toFixed(2);
      } else {
        return (this.fboTotal - this.homeTotal).toFixed(2);
      }
    },
  },
};
</script>
<style lang="scss" scoped>
.HOMECONTAINER {
  display: grid;
  height: 100vh;
  grid-template-rows: min-content min-content 7vh 1fr 10vh;
  background: var(--dark-background);
}
.INPUTCONTAINER {
  border: var(--green) 2px solid;
  border-radius: 10px;
}
.inputBox {
  display: flex;
  justify-content: space-between;
}
.FUELCONTAINER {
  display: flex;
  justify-content: space-evenly;
}
.verticalInput {
  display: flex;
  flex-direction: column;
}
.RESULTSCONTAINER {
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  grid-row: 4;
}
input {
  font-size: 1.7em;
  max-width: 2em;
}
.dollarSign {
  font-size: 2em;
}
.v-btn {
  color: var(--font-color);
}
</style>
