<template>
  <div class="wrapper">
    <SearchBar />
    <!-- <Map />   -->
  </div>
</template>

<script>
import Map from "./components/Map";
import SearchBar from "./components/SearchBar";

export default {
  components: {
    Map,
    SearchBar,
  }
};
</script>

<style >
/* .wrapper{
  display: flex;
  justify-content: center;
} */

#map{
  z-index: 0;
}

</style>