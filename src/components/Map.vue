<template>
  <div>
    <div id="map"></div>
  </div>
</template>

<script>
import L from "leaflet";
import "../../node_modules/leaflet.markercluster/dist/leaflet.markercluster.js";
import AreaList from "../../src/assets/data.json";

delete L.Icon.Default.prototype._getIconUrl;

L.Icon.Default.mergeOptions({
  iconRetinaUrl: require("leaflet/dist/images/marker-icon-2x.png"),
  iconUrl: require("leaflet/dist/images/marker-icon.png"),
  shadowUrl: require("leaflet/dist/images/marker-shadow.png")
});

export default {
  data: () => ({
    map: null,
    tileLayer: null,
    marker: null,
    areaList: null,
  }),

  methods: {
    initMap() {
      this.areaList = AreaList;
      this.map = L.map("map").setView([37.5660649, 126.982373], 13);
      this.tileLayer = L.tileLayer("http://{s}.tile.osm.org/{z}/{x}/{y}.png", {
        maxZoom: 18
      });
      this.tileLayer.addTo(this.map);
    },
    initLayers() {
      let markers = L.markerClusterGroup();
      for (let i = 0; i < this.areaList.length; i++) {
        let subwayIcon = L.icon({
          iconUrl: require(`../assets/${this.areaList[i].subwayLine}.png`),
          iconSize: [20, 20]
        });
        this.marker = L.marker(
          [
            Number(this.areaList[i].longitude),
            Number(this.areaList[i].latitude)
          ],
          { icon: subwayIcon }
        ).addTo(this.map);
        this.marker.bindTooltip(
          `<b>${this.areaList[i].subwayLine} ${this.areaList[i].subwayName}</b><br />${this.areaList[i].address}`
        );

        markers.addLayer(this.marker);
      }
      this.map.addLayer(markers);
    }
  },
  mounted() {
    this.initMap();
    this.initLayers();
  },
  computed: {
    selectTarget(){
      return this.$store.state.target;
    }
  },
  watch: {
    selectTarget: function(target) {
      console.log(target)
      if (target !== null) {
        let targetInfo = target.split(" ");
        let location = this.areaList.find(
          area =>
            area.subwayLine === targetInfo[0] &&
            area.subwayName === targetInfo[1]
        );
        this.map.flyTo(
          [Number(location.longitude), Number(location.latitude)],
          15
        );
      }
    }
  }
};
</script>

<style>
@import "../../node_modules/leaflet.markercluster/dist/MarkerCluster.css";
@import "../../node_modules/leaflet.markercluster/dist/MarkerCluster.Default.css";
@import "../../node_modules/leaflet/dist/leaflet.css";

#map {
  height: 937px; /* height: 100% 하면 지도가 아예 안보여요*/
  width: 100%;
}
</style>