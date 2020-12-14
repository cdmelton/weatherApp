<template>
  <div id="app">
    <Header />
    <Input @clicked="changeZip" />
    <Weather :zip="zip" :unit="unit" :key="zip + unit" />
  </div>
</template>


<script>
import Header from "./components/Header.vue";
import Input from "./components/Input.vue";
import Weather from "./components/Weather.vue";

export default {
  name: "App",
  components: {
    Header,
    Input,
    Weather,
  },
  data() {
    return {
      zip: "", // user zipcode to pass between components
      unit: "imperial", // user selected degree unit to pass between components
    };
  },
  methods: {
    // change vars globally
    changeZip(zip, unit) {
      this.zip = zip;
      this.unit = unit;
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
}
</style>
