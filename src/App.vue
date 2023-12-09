<template>
  <v-app>
    <gis-map
      :latitude="55.751834296843505"
      :longitude="37.61911466717721"
      :propZoom="12"
    ></gis-map>
  </v-app>
</template>

<script lang="ts">
import axios from "axios";
import GisMap from "./components/GisMap.vue";

export default {
  name: "app",
  components: {
    GisMap,
  },
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
