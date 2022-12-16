<script setup>
import * as d3 from 'd3';
import { onMounted, ref, computed } from 'vue';

import * as L from 'leaflet';
// very important
import 'leaflet/dist/leaflet.css';
import states_json from '../assets/states.json';



onMounted (() => {
  console.log(states_json);
  // var map = new leaflet.Map("map", {center: [60.8, -96.9], zoom: 4})
  //   .addLayer(new leaflet.TileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png"));
  // var svg = d3.select(map.getPanes().overlayPane).append("svg"),
  //   g = svg.append("g").attr("class", "leaflet-zoom-hide");
  var map = L.map('map').setView([49.905, -97.13], 10);
  L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    maxZoom: 15,
    attribution: '© OpenStreetMap'
  }).addTo(map);
  var svg = d3.select(map.getPanes().overlayPane).append("svg"),
    g = svg.append("g").attr("class", "leaflet-zoom-hide");
  var marker = L.marker([49.905, -97.13]).addTo(map);
  var popper = marker.bindPopup("<b>Hello world!</b><br>I am a popup.")//.openPopup();
  marker.on('mouseover',function(ev) {
    ev.target.openPopup();
  });
  marker.on('mouseout',function(ev) {
    ev.target.closePopup();
  });
  
})
</script>

<template>
  <div id="map"></div>
</template>