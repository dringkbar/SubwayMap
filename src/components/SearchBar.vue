<template>
  <v-app id="inspire">
    <div>
      <v-autocomplete 
        v-model="target" 
        label="검색어를 입력해 주세요" 
        :items="components"
      ></v-autocomplete>
      <Map
        :target ="target" 
      />
    </div>
  </v-app>
</template>

<script>
import AreaList from "../../src/assets/data.json";
import Map from "./Map";

export default {
  components: {
    Map
  },
  data() {
    return {
      areaList: AreaList,
      components: [],
      target:null
    };
  },
  mounted() {
    this.setAutoCompleteData();
  },
  methods: {
    setAutoCompleteData() {
      for (let i = 0; i < this.areaList.length; i++) {
        this.components.push(`${this.areaList[i].subwayLine} ${this.areaList[i].subwayName}`);
      }
    },
  }
};
</script>

<style>
/* #inspire {
  display: flex;
  justify-content: center;
  height: 70px;
  z-index: 1;
  position: fixed;
  margin-top: 10px;
  left: 100px;
  padding: 10px 5px 10px;
  border-radius: 10px;
} */

/* #inspire{
  height: 70px;
} */

</style>