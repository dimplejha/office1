
<template>
  <div>
    <div class="main-div">
      <div class="left-div">
        <h3></h3>
        <ul>
          <li v-for="(item, index) in state.geomData" :key="index">
            <input type="checkbox" @click="getdata(index)" />
            {{ item }}
          </li>
        </ul>
      </div>
      <main class="right-div">
        <v-map
          class="w-full h-full"
          :options="state.map"
          @loaded="onMapLoaded"
        ></v-map>
      </main>
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
    center: [80.93902587890625, 26.841533092305998],
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
    console.log("hii", e.features[0].geometry);
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
html,
body {
  margin: 0%;
}
.left-div {
  width: 35%;
  background-color: rgb(119, 83, 138);
  /* padding-right: 100px; */
  margin: 10px;
}
.right-div {
  width: 75%;
  height: 100vh;
}
.main-div {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}
div {
  border: solid black 1px;
}
li {
  /* margin: 200px; */
  padding-right: 100px;
}
</style>




