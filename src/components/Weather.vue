<template>
  <div class="box" ref="box">
    <div v-bind:key="x.id" v-for="x in current" class="card">
      <img v-bind:src="x.icon" alt="Card image cap" class="icon" />
      <img v-bind:src="x.icon" alt="Card image cap" class="shadow" />
      <div class="card-body">
        <h3 class="card-title">{{ x.city }}</h3>
        <p class="card-text">{{ x.cond }}</p>
        <p class="card-text">{{ x.temp + x.CF }}</p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios"; // for api import


export default {
  name: "Weather",
  data() {
    return {
      aryData: [], // ary for api data
      current: [], // chosen units
      imgUrl: "http://openweathermap.org/img/wn/", // img src for api icon
      // apiKey : "",
      degree: "K", // default unit identifier
      lat: 0,
      long: 0,
      loc: "",
    };
  },

  // grab api data
  mounted() {
    var noLoc =
      "https://api.openweathermap.org/data/2.5/weather?zip=" +
      this.zip +
      ",us&appid=047b14dc40c95da280f706a8a42c17e2" +
      "&units=" +
      this.unit;
    var yesLoc =
      "https://api.openweathermap.org/data/2.5/weather?lat=" +
      this.lat +
      "&lon=" +
      this.long +
      "&appid=047b14dc40c95da280f706a8a42c17e2" +
      "&units=" +
      this.unit;
    this.getLoc(noLoc, yesLoc);

    axios.get(this.Loc).then((response) => {
      // set unit identifier
      if (this.unit == "imperial") {
        this.degree = "F";
      } else if (this.unit == "metric") {
        this.degree = "C";
      }

      // use returned data
      this.aryData = response.data;
      this.parse(this.aryData);
    });
  },

  methods: {
    getLoc(no, yes) {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(this.displayPosition);
        this.loc = yes;
      } else {
        this.loc = no;
      }
    },
    displayPosition(position) {
      this.lat = position.coords.latitude;
      this.long = position.coords.longitude;
      alert(this.loc);
    },

    parse(aryData) {
      // add item to array
      this.current.push({
        id: aryData.id,
        city: aryData.name,
        temp: Math.round(aryData.main.temp),
        cond: aryData.weather[0].description,
        icon: this.imgUrl + aryData.weather[0].icon + "@2x.png",
        CF: this.degree,
      });
    },
  },
  props: ["zip", "unit"],
};
</script>

<style scoped>
.box {
  position: relative;
  width: 100%;
  margin: auto;
  margin-top: -8%;
  bottom: 3%;
  z-index: -1;
}
.card {
  display: block;
  width: 50%;
  height: 100%;
  padding: 1%;
  padding-top: 0;
  text-align: center;
  margin: auto;
}
h3 {
  font-weight: 500;
  font-size: 2em;
  line-height: 1.5em;
}
p {
  font-size: 2em;
}
.icon {
  position: relative;
  width: 90%;
  height: 50%;
  z-index: 1;
}
.shadow {
  display: block;
  width: 110%;
  position: relative;
  margin-top: -100.8%;
  margin-left: -5.17%;
  filter: blur(10px);
  filter: invert(100%);
  z-index: 0;
}
.card-body {
  margin-top: -25%;
  padding-bottom: 0%;
}
</style>