<template>
  <div>
    <!-- <div style="height: 200px overflow: auto;">
      <p>First marker is placed at {{ withPopup.lat }}, {{ withPopup.lng }}</p>
      <p>Center is at {{ currentCenter }} and the zoom is: {{ currentZoom }}</p>
      <button @click="showLongText">Toggle long popup</button>
      <button @click="showMap = !showMap">Toggle map</button>
    </div> -->
    <no-ssr>
      <l-map
        v-if="showMap"
        :zoom="zoom"
        :center="center"
        class="vue-map-container"
        :options="mapOptions"
        @update:center="centerUpdate"
        @update:zoom="zoomUpdate"
      >
        <l-tile-layer :url="url" :attribution="attribution" />
        <l-marker :lat-lng="withPopup">
          <l-popup>
            <div @click="innerClick">
              I am a popup
              <p v-show="showParagraph">
                Lorem ipsum dolor sit amet, consectetur adipiscing elit. Quisque
                sed pretium nisl, ut sagittis sapien. Sed vel sollicitudin nisi.
                Donec finibus semper metus id malesuada.
              </p>
            </div>
          </l-popup>
        </l-marker>
        <l-marker :lat-lng="withTooltip">
          <l-tooltip :options="{ permanent: true, interactive: true }">
            <div @click="innerClick">
              I am a tooltip
              <p v-show="showParagraph">
                Lorem ipsum dolor sit amet, consectetur adipiscing elit. Quisque
                sed pretium nisl, ut sagittis sapien. Sed vel sollicitudin nisi.
                Donec finibus semper metus id malesuada.
              </p>
            </div>
          </l-tooltip>
        </l-marker>
      </l-map>
    </no-ssr>
  </div>
</template>

<script>
let leaflet;
if (process.client) {
  leaflet = require("leaflet");
}
let Vue2Leaflet = {};

if (process.client) {
  Vue2Leaflet = require("vue2-leaflet");
}
// import { LMap, LTileLayer, LMarker, LPopup, LTooltip } from "vue2-leaflet";

export default {
  name: "Leaflet",
  components: {
    LMap: Vue2Leaflet.LMap,
    LTileLayer: Vue2Leaflet.LTileLayer,
    LMarker: Vue2Leaflet.LMarker,
    LPopup: Vue2Leaflet.LPopup,
    LTooltip: Vue2Leaflet.LTooltip,
  },
  // https://cartodb-basemaps-{s}.global.ssl.fastly.net/dark_all/{z}/{x}/{y}.png
  // https://tiles.stadiamaps.com/tiles/alidade_smooth_dark/{z}/{x}/{y}{r}.png
  data() {
    return {
      zoom: 10,
      center: process.client ? this.$L.latLng(37.9432, 23.6709) : null,
      url:
        "https://cartodb-basemaps-{s}.global.ssl.fastly.net/dark_all/{z}/{x}/{y}.png",
      attribution:
        '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      withPopup: process.client ? this.$L.latLng(47.41322, -1.219482) : null,
      withTooltip: process.client ? this.$L.latLng(47.41422, -1.250482) : null,
      currentZoom: 11.5,
      currentCenter: process.client
        ? this.$L.latLng(47.41322, -1.219482)
        : null,
      showParagraph: false,
      mapOptions: {
        zoomSnap: 0.5,
      },
      showMap: true,
    };
  },
  created() {
    console.log(this.withPopup);
  },
  methods: {
    zoomUpdate(zoom) {
      this.currentZoom = zoom;
    },
    centerUpdate(center) {
      this.currentCenter = center;
    },
    showLongText() {
      this.showParagraph = !this.showParagraph;
    },
    innerClick() {
      alert("Click!");
    },
  },
};
</script>

<style scoped lang="postcss">
.vue-map-container {
  position: fixed;
}
</style>
