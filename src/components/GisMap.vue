<template>
  <div id="map" style="width: 100%; height: 100%">
    <l-map
      v-if="ready"
      ref="map"
      v-model:zoom="zoom"
      :center="[latitude, longitude]"
    >
      <l-geo-json :geojson="geoJSON" :options="geojsonOptions" />
    </l-map>
  </div>
</template>

<script lang="ts">
import "2gis-maps";
import axios from "axios";
import { Vue, Options } from "vue-class-component";
import { LMap, LGeoJson } from "@vue-leaflet/vue-leaflet";

@Options({
  components: {
    LMap,
    LGeoJson,
  },
  props: {
    latitude: Number,
    longitude: Number,
    propZoom: Number,
  },
  beforeMount() {
    this.getURLFragment().then({});
    this.zoom = this.propZoom;
  },
  methods: {
    getURLFragment() {
      return axios
        .get(decodeURIComponent(window.location.hash.substr(1)))
        .then((response) => {
          console.log(response.data);
          this.geoJSON = response.data;
          this.ready = true;
        })
        .catch((err) => {
          console.error(err);
          this.geoJSON = {};
          this.ready = true;
        });
    },
  },
})
export default class GisMap extends Vue {
  zoom!: number;
  latitude!: number;
  longitude!: number;
  geoJSON!: any;
  ready: boolean = false;
  geojsonOptions: any = {
    onEachFeature: (f: any, l: any) => {
      l.bindPopup(
        '<pre style="height: 100px; overflow: scroll;">' +
          JSON.stringify(f.properties, null, " ")
            .replace(
              //eslint-disable-next-line
            /[\{\}"]/g,
              ""
            )
            .replaceAll("\\n", "<br/>") +
          "</pre>"
      );
    },
  };
}
</script>
