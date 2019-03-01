<template>
  <div>
    <b-container>
      <b-row class="justify-content-center">
        <b-col cols="6" class="house-col" :class="size">
          <House :size="size" :sunPos="sunPos"></House>
        </b-col>
      </b-row>
      <b-row class="justify-content-center">
        <b-col class="justify-content-center text-center">
          <b-input-group prepend="Cost of House" class="mt-3">
            <b-form-input type="number" class="text-right" v-model="p" />
            <b-input-group-text slot="append">
              <strong class="text-success">USD</strong>
            </b-input-group-text>
          </b-input-group>
          <b-form-input
            class="p-3"
            type="range"
            v-model="p"
            min="100000"
            max="5000000"
            step="10000"
          />
          <b-row>
            <b-col class="text-center justify-content-center">
              <b-button-group class="text-center" vertical size="sm">
                <b-button @click="p = 150000">$150,000</b-button>
                <b-button @click="p = 250000">$250,000</b-button>
                <b-button @click="p = 500000">$500,000</b-button>
              </b-button-group>
            </b-col>

            <b-col></b-col>
          </b-row>
        </b-col>
        <b-col class="justify-content-center text-center">
          <b-input-group prepend="APR Rate" class="mt-3">
            <b-form-input type="number" class="text-right" v-model="r" />
            <b-input-group-text slot="append">
              <strong class="text-success">%</strong>
            </b-input-group-text>
          </b-input-group>
          <b-form-input
            class="p-3"
            type="range"
            v-model="r"
            min="1"
            max="15"
            step=".1"
          />
        </b-col>
        <b-col class="justify-content-center text-center">
          <b-input-group prepend="Term Length" class="mt-3">
            <b-form-input type="number" class="text-right" v-model="n" />
            <b-input-group-text slot="append">
              <span class="text-grey">Years</span>
            </b-input-group-text>
          </b-input-group>
          <b-form-input
            class="p-3"
            type="range"
            v-model="n"
            min="15"
            max="45"
          />
        </b-col>
      </b-row>
      <b-row>
        <b-col class="text-center">
          <hr />
          Your Monthly Payment
          <h2 class="lead">${{ monthlyPayment }}</h2>
          <h4>{{ sunPos }}</h4>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import House from "./House";
export default {
  name: "Fields",
  components: {
    House
  },
  data: function() {
    return {
      p: 150000,
      r: 6.5,
      n: 30
    };
  },
  computed: {
    // @param p float Amount borrowed
    // @param r interest as percentage
    // @param n time in years
    // c = ((p * r) * Math.pow((1 + r), n)) / (Math.pow((1 + 4 ), n) - 1)

    monthlyPayment() {
      // convert rate to decimal
      let rate = this.r / 12 / 100;

      // convert time from years to months
      let years = this.n * 12;

      let cost = this.p;

      const pmt = (rate * cost) / (1 - Math.pow(1 + rate, -years));
      return pmt.toFixed(2);
    },

    size() {
      const values = [150000, 200000, 500000];
      let size = {};
      if (this.p <= values[0]) {
        size = "sm";
      } else if (this.p > values[0] && this.p < values[2]) {
        size = "md";
      } else if (this.p >= values[2]) {
        size = "lg";
      }
      return size;
    },
    sunPos() {
      return ((this.r / 15) * 100 - 100) * -1;
    }
  }
};
</script>

<style>
.house-col.sm {
  margin-left: 145px;
  transition: 0.5s;
}

.house-col.md,
.house-col.lg {
  margin-left: 0px;
  transition: 0.5s;
}
</style>
