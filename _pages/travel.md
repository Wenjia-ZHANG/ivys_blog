---
layout: single
title: "Travel & Photos"
permalink: /travel/
classes: wide
author_profile: true
---

### 🗺️ Ivy's Travel Map  
Click markers or use the sidebar list to explore trips.

{% assign travel_base = "/travel/" | relative_url %}

<style>
  .travel-container {
    display: flex;
    gap: 20px;
  }
  .travel-sidebar {
    width: 260px;
    max-height: 550px;
    overflow-y: auto;
    padding: 10px 15px;
    border-radius: 8px;
    background: #f7f7f7;
    border: 1px solid #ddd;
  }
  .travel-sidebar h2 {
    margin-top: 0;
    font-size: 1.2em;
  }
  .travel-item {
    padding: 8px 10px;
    margin-bottom: 8px;
    border-radius: 6px;
    cursor: pointer;
    transition: background 0.25s;
  }
  .travel-item:hover {
    background: #e3efff;
  }
  .travel-item.active {
    background: #c7ddff;
    font-weight: bold;
  }
  #travel-map {
    flex: 1;
    height: 550px;
    border-radius: 8px;
  }
  @media(max-width: 850px) {
    .travel-container { flex-direction: column; }
    .travel-sidebar { width: 100%; max-height: none; }
    #travel-map { height: 400px; }
  }
</style>

<div class="travel-container">

  <!-- SIDEBAR -->
  <div class="travel-sidebar">
    <h2>Destinations</h2>

    <div class="travel-item" data-loc="zion">🏜️ Zion National Park</div>
    <div class="travel-item" data-loc="xinjiang">🏔️ Xinjiang, China</div>
    <div class="travel-item" data-loc="newyork">🗽 New York City</div>
    <div class="travel-item" data-loc="yellowstone">🦬 Yellowstone</div>
    <div class="travel-item" data-loc="teton">⛰️ Grand Teton</div>
    <div class="travel-item" data-loc="vegas">🎰 Las Vegas</div>
    <div class="travel-item" data-loc="rainier">🏞️ Mount Rainier</div>
    <div class="travel-item" data-loc="olympic">🌲 Olympic National Park</div>
    <div class="travel-item" data-loc="hawaii">🏝️ Hawaii</div>

    <div class="travel-item" data-loc="london">🇬🇧 London</div>
    <div class="travel-item" data-loc="toronto">🇨🇦 Toronto</div>
    <div class="travel-item" data-loc="orlando">🎢 Orlando</div>
    <div class="travel-item" data-loc="montreal">🍁 Montreal</div>
  </div>

  <!-- MAP -->
  <div id="travel-map"></div>
</div>

<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css" />
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/Leaflet.awesome-markers/2.0.2/leaflet.awesome-markers.css" />

<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/Leaflet.awesome-markers/2.0.2/leaflet.awesome-markers.min.js"></script>

<script>
var cameraIcon = L.AwesomeMarkers.icon({
  icon: 'camera',
  prefix: 'fa',
  markerColor: 'blue',
  iconColor: 'white'
});

var hometownIcon = L.AwesomeMarkers.icon({
  icon: 'home',
  prefix: 'fa',
  markerColor: 'red',
  iconColor: 'white'
});

var pittsIcon = L.icon({
  iconUrl: 'https://raw.githubusercontent.com/pointhi/leaflet-color-markers/master/img/marker-icon-yellow.png',
  iconSize: [25, 41],
  iconAnchor: [12, 41],
  popupAnchor: [1, -34],
  shadowUrl: 'https://raw.githubusercontent.com/pointhi/leaflet-color-markers/master/img/marker-shadow.png',
  shadowSize: [41, 41]
});

var map = L.map("travel-map").setView([39.5, -98.35], 4);

L.tileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png", {
  attribution: "&copy; OpenStreetMap contributors",
}).addTo(map);

var locations = {
  zion:        { name: "Zion National Park", coords: [37.2982, -113.0263], url: "{{ travel_base }}zion/", icon: cameraIcon },
  xinjiang:    { name: "Xinjiang, China", coords: [43.7928, 87.6177], url: "{{ travel_base }}xinjiang/", icon: cameraIcon },
  newyork:     { name: "New York City", coords: [40.7128, -74.0060], url: "{{ travel_base }}newyork/", icon: cameraIcon },
  yellowstone: { name: "Yellowstone National Park", coords: [44.4280, -110.5885], url: "{{ travel_base }}yellowstone/", icon: cameraIcon },
  teton:       { name: "Grand Teton", coords: [43.7904, -110.6818], url: "{{ travel_base }}teton/", icon: cameraIcon },
  vegas:       { name: "Las Vegas", coords: [36.1699, -115.1398], url: "{{ travel_base }}vegas/", icon: cameraIcon },
  rainier:     { name: "Mount Rainier", coords: [46.8523, -121.7603], url: "{{ travel_base }}rainier/", icon: cameraIcon },
  olympic:     { name: "Olympic National Park", coords: [47.8021, -123.6044], url: "{{ travel_base }}olympic/", icon: cameraIcon },
  hawaii:      { name: "Hawaii", coords: [19.8968, -155.5828], url: "{{ travel_base }}hawaii/", icon: cameraIcon },

  london:      { name: "London", coords: [51.5072, -0.1276], url: "{{ travel_base }}london/", icon: cameraIcon },
  toronto:     { name: "Toronto", coords: [43.6532, -79.3832], url: "{{ travel_base }}toronto/", icon: cameraIcon },
  orlando:     { name: "Orlando", coords: [28.5383, -81.3792], url: "{{ travel_base }}orlando/", icon: cameraIcon },
  montreal:    { name: "Montreal", coords: [45.5019, -73.5674], url: "{{ travel_base }}montreal/", icon: cameraIcon },

  shenyang:    { name: "Shenyang (Hometown)", coords: [41.8057, 123.4315], icon: hometownIcon },
  pittsburgh:  { name: "Pittsburgh", coords: [40.4406, -79.9959], icon: pittsIcon }
};

var markers = {};

Object.keys(locations).forEach(function (key) {
  var loc = locations[key];
  var options = loc.icon ? { icon: loc.icon } : {};
  var marker = L.marker(loc.coords, options).addTo(map);

  if (loc.url) {
    marker.bindPopup(`<b>${loc.name}</b><br><a href="${loc.url}">View trip →</a>`);
  } else {
    marker.bindPopup(`<b>${loc.name}</b>`);
  }

  markers[key] = marker;

  marker.on("click", function () {
    highlightSidebar(key);
  });
});

function highlightSidebar(key) {
  document.querySelectorAll(".travel-item").forEach(el => el.classList.remove("active"));
  var item = document.querySelector(`.travel-item[data-loc="${key}"]`);
  if (item) item.classList.add("active");
}

document.querySelectorAll(".travel-item").forEach(el => {
  el.addEventListener("click", function () {
    var key = this.getAttribute("data-loc");
    var loc = locations[key];
    map.setView(loc.coords, 6, { animate: true });
    markers[key].openPopup();
    highlightSidebar(key);
  });
});
</script>

*Last updated: {{ site.time | date: "%B %Y" }}*
