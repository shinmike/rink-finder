<template>
  <div>
    <nav class="dt w-100 border-box ph5-ns shadow-5 bg-green black-70">
      <h1 class="f2">Rink Finder <i class="fa fa-search" aria-hidden="true"></i></h1>
      <div class="dtc v-mid w-75 tr">
        <form>
          <select v-model="selected">
            <option v-for="category in categories">{{category}}</option>
          </select>
          <button class="f6 grow no-underline br-pill ba ph3 pv2 mb2 dib black" v-on:click.prevent="runData">Search</button>
        </form>
      </div>
    </nav>
    <indoor-rinks v-bind:indoorRinkData="data" v-if="indoorRinks"></indoor-rinks>
    <outdoor-rinks v-bind:outdoorRinkData="data" v-if="outdoorRinks"></outdoor-rinks>
    <google-maps v-bind:indoorRinkData="data" v-if="indoorRinks"></google-maps>
    <google-maps v-bind:outdoorRinkData="data" v-if="outdoorRinks"></google-maps>
  </div>
</template>

<script>
import indoorRinks from "./indoorRinks.vue";
import outdoorRinks from "./outdoorRinks.vue";
import googleMaps from "./googleMaps.vue";

export default {
  components: {
    "indoor-rinks": indoorRinks,
    "outdoor-rinks": outdoorRinks,
    "google-maps": googleMaps
  },
  data() {
    return {
      selected: "indoor_rinks",
      categories: ["indoor_rinks", "outdoor_rinks"],
      data: [],
      indoorRinks: true,
      outdoorRinks: false
    };
  },
  mounted: function() {
    let url =
      "https://api-us.hockey-community.com/v1/explore/locations?category=indoor_rinks&sw=49.1407,-123.0899&ne=49.3261,-122.9310&origin=49.2335,-123.0104";
    this.$http.get(url).then(
      function(response) {
        console.log("SUCCESS", response);
        this.data = response.data;
      },
      function(error) {
        console.log("ERROR", error);
      }
    );
  },

  methods: {
    runData() {
      let url =
        "https://api-us.hockey-community.com/v1/explore/locations?category=" +
        this.selected +
        "&sw=49.1407,-123.0899&ne=49.3261,-122.9310&origin=49.2335,-123.0104";
      if (this.selected === "indoor_rinks") {
        this.indoorRinks = true;
        this.outdoorRinks = false;
      }
      if (this.selected === "outdoor_rinks") {
        this.outdoorRinks = true;
        this.indoorRinks = false;
      }
      this.$http.get(url).then(
        function(response) {
          console.log("SUCCESS", response);
          this.data = response.data;
        },
        function(error) {
          console.log("ERROR", error);
        }
      );
    }
  }
};
</script>

<style>

nav{
  background-color: #fff;
  border-top: 2px solid #E0E1E1;
  box-shadow: 0px 3px 5px #b5b5b5;
  z-index: 9999;
}

button {
  margin-left: 10px;
}

</style>