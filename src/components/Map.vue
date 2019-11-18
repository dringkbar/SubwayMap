<template>
  <div>
    {{target}}
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
  props: {
    target: String
  },
  data: () => ({
    map: null,
    tileLayer: null,
    marker: null,
    areaList: null
  }),
  methods: {
    initMap() {
      this.areaList = AreaList;
      this.map = L.map("map").setView([37.5660649, 126.982373], 13);
      this.tileLayer = L.tileLayer("http://{s}.tile.osm.org/{z}/{x}/{y}.png", {
        maxZoom: 18
      });

      this.tileLayer.addTo(this.map);

      this.setIcon();
    },

    initLayers() {
      this.layers.forEach(layer => {
        const markerFeatures = layer.features.filter(
          feature => feature.type === "marker"
        );
        const polygonFeatures = layer.features.filter(
          feature => feature.type === "polygon"
        );

        markerFeatures.forEach(feature => {
          feature.leafletObject = L.marker(feature.coords).bindPopup(
            feature.name
          );
        });

        polygonFeatures.forEach(feature => {
          feature.leafletObject = L.polygon(feature.coords).bindPopup(
            feature.name
          );
        });
      });
    },
    setIcon() {
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
      }
    }
  },
  mounted() {
    this.initMap();
    // this.initLayers();
  },
  watch: {
    target: function() {
      if (this.target !== null) {
        for (let i = 0; i < this.areaList.length; i++) {
          if (this.target === this.areaList[i].address)
            this.map.flyTo(
              [ Number(this.areaList[i].longitude), Number(this.areaList[i].latitude)], 15);
        }
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