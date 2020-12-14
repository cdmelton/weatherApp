<template>
  <div class="box" ref="box">
    <div class="info" ref="hold"><h1>Enter a Zipcode</h1></div>
    <div class="container" @keyup.enter="verify()" ref="search">
      <input
        type="text"
        ref="code"
        name="zip"
        placeholder="Zip Code"
        class="zip"
      />
      <div class="radio">
        <input
          type="radio"
          id="c"
          name="unit"
          value="c"
          @click="setUnit('0')"
          class="unit"
        />
        <label for="c" class="unit">C</label>
        <input
          type="radio"
          id="f"
          name="unit"
          value="f"
          @click="setUnit('1')"
          class="unit"
          checked
        />
        <label for="f" class="unit">F</label>
        <img
          src="@/assets/searchIcon.png"
          alt="Search"
          class="icon"
          @click="verify()"
        />
      </div>
    </div>
    <!-- <button type="button" @click="verify()" id="btn">Search</button> -->
  </div>
</template>

<script>
export default {
  name: "Input",
  data() {
    return {
      unit: "imperial",
      // set units selected by user
      setUnit(val) {
        if (val == 0) {
          this.unit = "metric";
        } else {
          this.unit = "imperial";
        }
      },

      // verify entered zipcode
      verify() {
        var zip = this.$refs.code.value; // set zipcode to input value
        var isValidZip = /^[0-9]{5}(?:-[0-9]{4})?$/.test(zip); // test is zipcode format is valid

        //if valid zipcode- send to parent for component refresh
        if (isValidZip) {
          this.toggleCSS();
          this.$emit("clicked", zip, this.unit);
        } else {
          alert("Please enter a valid zipcode. (ex. 12345)");
        }
      },

      toggleCSS() {
        this.$refs.box.style.position = "relative";
        this.$refs.box.style.left = "0";
        this.$refs.box.style.top = "0";
        this.$refs.box.style.marginTop = "0";
        this.$refs.box.style.marginLeft = "-30%";
        this.$refs.hold.style.display = "none";
        this.$refs.search.paddingLeft = "0";
      },
    };
  },
};
</script>

<style scoped>
.container {
  display: block;
  margin: auto;
  width: 40%;
  padding-left: 13%;
}
.box {
  display: block;
  position: relative;
  width: 100%;
  margin: auto;
  margin-top: 15%;
}
.zip {
  width: 50%;
  padding: 0.5%;
  font-size: 110%;
  display: inline-block;
}
.unit {
  padding-right: 0.5%;
  font-size: 120%;
  width: 15%;
}
.radio,
.icon {
  display: inline-block;
  white-space: nowrap;
}
.radio {
  width: 20%;
}
.icon {
  width: 30%;
  padding-left: 10%;
}
.info {
  text-align: center;
}
</style>