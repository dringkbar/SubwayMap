<template>
  <v-app id="inspire">
    <div>
      <v-autocomplete label="검색어를 입력해 주세요" :items="components"></v-autocomplete>
    </div>
  </v-app>
</template>

<script>
import AreaList from "../../src/assets/data.json";

export default {
  data() {
    return {
        areaList: AreaList,
      components: [
          "1호선",
          "2호선",
          "3호선",
          "4호선",
          "5호선",
          "6호선",
          "7호선",
          "8호선",
          "9호선",
          "분당선",
          "신분당선",
      ]
    };
  },
  mounted() {
      this.setAutoCompleteData()
  },
  methods:{
      setAutoCompleteData(){
          for (let i = 0; i < this.areaList.length; i++) {
             this.components.push(this.areaList[i].subwayName)
             this.components.push(this.areaList[i].courtBuilding)
             this.components.push(this.areaList[i].address)
          }
      }
  }
};
</script>

<style>
#inspire {
  display: flex;
  justify-content: center;
  height: 70px;
  z-index: 1;
  position: fixed;
  margin-top: 10px;
  left: 100px;
  padding: 10px 5px 10px;
  border-radius: 10px;
}
</style>