<template>
  <div>
    <div class="main-div">
      <div class="left-div">
        <nav id="menu"></nav>

        <h3></h3>
        <ul>
          <li v-for="(item, index) in state.data" :key="index">
            <input type="checkbox" @click="getdata(index)" />
            {{ item.name }}
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
    center: [80.93902587890625, 26.841533092305998],
    zoom: 11,
    maxZoom: 22,
    crossSourceCollisions: false,
    failIfMajorPerformanceCaveat: false,
    attributionControl: false,
    preserveDrawingBuffer: true,
    hash: false,
    minPitch: 0,
    maxPitch: 60,
  },
  data: [],
  layersName: "",
});
function getdata(e) {
  console.log(state.data[0]);
}
function onMapLoaded(map: mapboxgl.Map) {
  var Draw = new MapboxDraw();
  map.addControl(Draw, "top-right");
  map.on("draw.create", updateArea);
  map.addControl(new mapboxgl.FullscreenControl());
  map.addControl(new mapboxgl.NavigationControl(), "top-left");

  function updateArea(e) {
    console.log("clicked");
    const data = Draw.getAll();
    data1.data2.push(data);
    console.log(data["features"][0]["geometry"]["coordinates"][0]);
    map.on("click", "gl-draw-polygon-fill.cold", function (e) {
      var description =
        "<form> Enter name: <input type='text'/></br> Enter Description:<input type='description'/><button id='view-full'>Submit</button></form > ";
      new mapboxgl.Popup().setLngLat(e.lngLat).setHTML(description).addTo(map);
    });
  }

  // function updateArea(e) {
  //   console.log(e.features[0].geometry);

  //   let name = prompt("Enter the name ");
  //   let coordinates = e.features[0].geometry.coordinates;
  //   let type1 = e.features[0].geometry.type;
  //   let data1 = {
  //     name: `${name}`,
  //     type: `${type1}`,
  //     coordinates: `${coordinates}`,
  //   };

  //   // const popup = new mapboxgl.Popup()
  //   //   .setLngLat(coordinates)
  //   //   .setHTML(`<input type="text" v-model="data.layearName"/> `)
  //   //   .addTo(map);
  //   // data push in array
  //   state.data.push(data1);
  //   const popup = new mapboxgl.Popup({ closeOnClick: false })
  //     .setLngLat([-96, 37.8])
  //     .setHTML("<h1>Hello World!</h1>")
  //     .addTo(map);
  //   console.log(coordinates);
  //   console.log(type1);
  // }
  console.log(state.data);
}
</script>
<style>
.left-div {
  height: 100vh;
  width: 19vw;
  background-color: rgb(182, 166, 191);
  float: left;
}
.right-div {
  height: 100vh;
  width: 81vw;
  float: right;
}
.main-div {
  height: 100vh;
  width: 100vw;
}

#menu {
  background: #fff;
  position: absolute;
  z-index: 1;
  top: 10px;
  right: 10px;
  border-radius: 3px;
  width: 120px;
  border: 1px solid rgba(0, 0, 0, 0.4);
  font-family: "Open Sans", sans-serif;
}

#menu a {
  font-size: 13px;
  color: #404040;
  display: block;
  margin: 0;
  padding: 0;
  padding: 10px;
  text-decoration: none;
  border-bottom: 1px solid rgba(0, 0, 0, 0.25);
  text-align: center;
}

#menu a:last-child {
  border: none;
}

#menu a:hover {
  background-color: #f8f8f8;
  color: #404040;
}

#menu a.active {
  background-color: #3887be;
  color: #ffffff;
}

#menu a.active:hover {
  background: #3074a4;
}
</style>








