<template>
  <div>
    <div class="main-div">
      <div class="left-div">
        List Of Data
        <button class="flyToIcon">i</button>
        <div class="calculation-box">
          <p>Click the map to draw a polygon.</p>
          <div id="calculated-area"></div>
        </div>
      </div>
      <div class="right-div">
        <pre id="info"></pre>

        <v-map :options="state.map" @loaded="onMapLoaded"></v-map>
      </div>
    </div>
  </div>
</template>
<script setup lang="ts">
import VMap from "v-mapbox";
import mapboxgl from "mapbox-gl";
import MapboxDraw from "@mapbox/mapbox-gl-draw";
import MapboxGeocoder from "@mapbox/mapbox-gl-geocoder";

const state: any = reactive({
  map: {
    accessToken:
      "pk.eyJ1Ijoic29jaWFsZXhwbG9yZXIiLCJhIjoiREFQbXBISSJ9.dwFTwfSaWsHvktHrRtpydQ",
    style: "mapbox://styles/mapbox/streets-v11?optimize=true",
    center: [444.04931277036667, 26.266912177018096] as number[], //uses longitude, latitude
    zoom: 1,
    // maxZoom: 22,
  },
  data2: [],
});

function onMapLoaded(map: mapboxgl.Map) {
  const geocoder = new MapboxGeocoder({
    accessToken:
      "pk.eyJ1Ijoic29jaWFsZXhwbG9yZXIiLCJhIjoiREFQbXBISSJ9.dwFTwfSaWsHvktHrRtpydQ",
    marker: {
      color: "orange",
    },
    mapboxgl: mapboxgl,
  });
  map.addControl(geocoder);

  var Draw = new MapboxDraw();
  map.addControl(Draw, "top-right");
  map.on("draw.create", updateArea);
  // map.on('draw.delete', updateArea);
  // map.on('draw.update', updateArea);
  // function updateArea(e) {
  //   const data = Draw.getAll();
  //   // alert(e["features"][0]["geometry"]["coordinates"][0]);
  //   const answer = document.getElementById("calculated-area");
  //   console.log(answer);
  //   answer.innerHTML = e["features"][0]["geometry"]["coordinates"][0];
  // }

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
  function handlePop() {
    console.log("Helo");
  }

  map.addControl(new mapboxgl.FullscreenControl());
  map.addControl(new mapboxgl.NavigationControl(), "top-left");
  // map.on("mousemove", (e) => {
  //   document.getElementById("info").innerHTML =
  //     // `e.point` is the x, y coordinates of the `mousemove` event
  //     // relative to the top-left corner of the map.
  //     JSON.stringify(e.point) +
  //     "<br />" +
  //     // `e.lngLat` is the longitude, latitude geographical position of the event.
  //     JSON.stringify(e.lngLat.wrap());
  // });
}
</script>
<style>
.left-div {
  height: 100vh;
  width: 20vw;
  background-color: #634bbc;
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
.flyToIcon {
  float: right;
}
#info {
  display: table;
  position: relative;
  margin: 0px auto;
  word-wrap: anywhere;
  white-space: pre-wrap;
  padding: 10px;
  border: none;
  border-radius: 3px;
  font-size: 12px;
  text-align: center;
  color: #222;
  background: #fff;
}

.mapboxgl-popup {
  max-width: 400px;
  font: 12px/20px "Helvetica Neue", Arial, Helvetica, sans-serif;
}
#geocoder-container > div {
  min-width: 50%;
  margin-left: 25%;
}
.calculation-box {
  height: auto;
  width: auto;
  position: absolute;
  /* bottom: 40px;
  left: 10px; */
  background-color: #51e2f5;
  padding: 10px;
  text-align: center;
}

p {
  font-family: "Open Sans";
  margin: 0;
  font-size: 13px;
}
</style>









