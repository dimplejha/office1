
<template>
  <div>
    <div class="main-div">
      <div class="left-div">
        <h3>
          <!-- <div>{{ state.geomData }}</div> -->
        </h3>
        <ul>
          <li v-for="(item, index) in state.geomData" :key="index">
            <input type="checkbox" @click="getdata(index)" />
            {{ item }}
          </li>
        </ul>
      </div>
      <div class="right-div">
        <v-map :options="state.map" @loaded="onMapLoaded"></v-map>
        <nav id="menu"></nav>
        <div id="map"></div>
      </div>
    </div>
  </div>
</template>
<script setup lang="ts">
import VMap from "v-mapbox";
import mapboxgl from "mapbox-gl";
import MapboxDraw from "@mapbox/mapbox-gl-draw";
const state = reactive({
  map: {
    accessToken:
      "pk.eyJ1IjoibWF5dXJ3YWtpa2FyIiwiYSI6ImNsNmdjdGxwbjBiNGMzY282bWh0dng2c2kifQ.y-m4-zQKOeOOnDG5I1u6ng",
    style: "mapbox://styles/mapbox/streets-v11?optimize=true",
    center: [77.222900390625, 28.65745356910103],
    zoom: 11,
  },
  geomData: [],
});
function onMapLoaded(map: mapboxgl.Map) {
  map.addControl(new mapboxgl.NavigationControl());
  map.addControl(new mapboxgl.FullscreenControl());

  var Draw = new MapboxDraw();
  map.addControl(Draw, "top-right");
  map.on("draw.create", updateArea);
  function updateArea(e) {
    // state.layearName = state.layearName ;
    console.log("hii dimple", e.features[0].geometry);
    // create object
    // let name = prompt("Enter the City Name ");
    let coordinates = e.features[0].geometry.coordinates;
    let types = e.features[0].geometry.type;
    let data = {
      // name: `${name}`,
      type: `${types}`,
      coordinates: `${coordinates}`,
    };

    // geomData push in array
    state.geomData.push(data);
    console.log(coordinates);
    console.log(types);

    map.on("click", function (e) {
      var description =
        "<form onSubmit='return false;'> Enter cityName: <input type='text'/></br> Enter discription:<input type='text'/><button id='view-full'>Submit</button></form > ";
      new mapboxgl.Popup().setLngLat(e.lngLat).setHTML(description).addTo(map);
    });
  }
}
console.log(state.geomData);
</script>
<style>
.left-div {
  height: 100vh;
  width: 19vw;
  background-color: rgb(252, 251, 253);
  float: left;
}
.right-div {
  height: 100vh;
  width: 80vw;
  float: right;
}
.main-div {
  height: 100vh;
  width: 100vw;
}
div {
  border: solid black 1px;
}
</style>




