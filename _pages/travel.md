---
layout: single
# title: "Travel Map"
permalink: /travel/
classes: wide
author_profile: true
---

# 🗺️ Interactive Travel Map  
Click markers or use the sidebar list to explore trips.

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

  <!-- SIDEBAR LIST -->
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

    <!-- also in sidebar -->
    <div class="travel-item" data-loc="london">🇬🇧 London</div>
    <div class="travel-item" data-loc="toronto">🇨🇦 Toronto</div>
    <div class="travel-item" data-loc="orlando">🎢 Orlando</div>
    <div class="travel-item" data-loc="montreal">🍁 Montreal</div>
  </div>

  <!-- MAP -->
  <div id="travel-map"></div>
</div>

<!-- Leaflet CSS -->
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />

<!-- Font Awesome (for camera icon) -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css" />

<!-- Leaflet Awesome Markers CSS -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/Leaflet.awesome-markers/2.0.2/leaflet.awesome-markers.css" />

<!-- Leaflet JS -->
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

<!-- Leaflet Awesome Markers JS -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/Leaflet.awesome-markers/2.0.2/leaflet.awesome-markers.min.js"></script>

<script>
// BLUE camera icon for sidebar destinations
var cameraIcon = L.AwesomeMarkers.icon({
  icon: 'camera',
  prefix: 'fa',
  markerColor: 'blue',
  iconColor: 'white'
});

// HOMETOWN special icon for Shenyang (house icon)
var hometownIcon = L.AwesomeMarkers.icon({
  icon: 'home',
  prefix: 'fa',
  markerColor: 'red',
  iconColor: 'white'
});

// Pittsburgh special icon (yellow pin)
var pittsIcon = L.icon({
  iconUrl: 'https://raw.githubusercontent.com/pointhi/leaflet-color-markers/master/img/marker-icon-yellow.png',
  iconSize: [25, 41],
  iconAnchor: [12, 41],
  popupAnchor: [1, -34],
  shadowUrl: 'https://raw.githubusercontent.com/pointhi/leaflet-color-markers/master/img/marker-shadow.png',
  shadowSize: [41, 41]
});

// Initialize map centered on US
var map = L.map("travel-map").setView([39.5, -98.35], 4);

L.tileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png", {
  attribution: "&copy; OpenStreetMap contributors",
}).addTo(map);

// ALL LOCATIONS
// ONLY sidebar destinations have URLs
var locations = {
  /* Sidebar → camera icon + clickable */
  zion:       { name: "Zion National Park", coords: [37.2982, -113.0263], url: "/travel/zion/", icon: cameraIcon },
  xinjiang:   { name: "Xinjiang, China", coords: [43.7928, 87.6177], url: "/travel/xinjiang/", icon: cameraIcon },
  newyork:    { name: "New York City", coords: [40.7128, -74.0060], url: "/travel/newyork/", icon: cameraIcon },
  yellowstone:{ name: "Yellowstone National Park", coords: [44.4280, -110.5885], url: "/travel/yellowstone/", icon: cameraIcon },
  teton:      { name: "Grand Teton National Park", coords: [43.7904, -110.6818], url: "/travel/teton/", icon: cameraIcon },
  vegas:      { name: "Las Vegas", coords: [36.1699, -115.1398], url: "/travel/vegas/", icon: cameraIcon },
  rainier:    { name: "Mount Rainier", coords: [46.8523, -121.7603], url: "/travel/rainier/", icon: cameraIcon },
  olympic:    { name: "Olympic National Park", coords: [47.8021, -123.6044], url: "/travel/olympic/", icon: cameraIcon },
  hawaii:     { name: "Hawaii", coords: [19.8968, -155.5828], url: "/travel/hawaii/", icon: cameraIcon },

  london:     { name: "London", coords: [51.5072, -0.1276], url: "/travel/london/", icon: cameraIcon },
  toronto:    { name: "Toronto", coords: [43.6532, -79.3832], url: "/travel/toronto/", icon: cameraIcon },
  orlando:    { name: "Orlando", coords: [28.5383, -81.3792], url: "/travel/orlando/", icon: cameraIcon },
  montreal:   { name: "Montreal", coords: [45.5019, -73.5674], url: "/travel/montreal/", icon: cameraIcon },

  /* MAP ONLY — NO LINK */
  dc:         { name: "Washington, D.C.", coords: [38.9072, -77.0369] },
  la:         { name: "Los Angeles", coords: [34.0522, -118.2437] },
  sandiego:   { name: "San Diego", coords: [32.7157, -117.1611] },
  boston:     { name: "Boston", coords: [42.3601, -71.0589] },
  chicago:    { name: "Chicago", coords: [41.8781, -87.6298] },
  austin:     { name: "Austin", coords: [30.2672, -97.7431] },

  seoul:      { name: "Seoul", coords: [37.5665, 126.9780] },
  tokyo:      { name: "Tokyo", coords: [35.6762, 139.6503] },

  /* HOMETOWN: Shenyang → special icon */
  shenyang:   { name: "Shenyang (Hometown)", coords: [41.8057, 123.4315], icon: hometownIcon },

  /* Pittsburgh → yellow icon */
  pittsburgh: { name: "Pittsburgh", coords: [40.4406, -79.9959], icon: pittsIcon }
};

// Create markers
var markers = {};

Object.keys(locations).forEach(function (key) {
  var loc = locations[key];

  // camera / hometown / pittsburgh icons
  var options = {};
  if (loc.icon) options.icon = loc.icon;

  var marker = L.marker(loc.coords, options).addTo(map);

  // Popup logic: only sidebar items have links
  if (loc.url) {
    marker.bindPopup(`<b>${loc.name}</b><br><a href="${loc.url}">View trip →</a>`);
  } else {
    marker.bindPopup(`<b>${loc.name}</b>`); // NO hyperlink
  }

  markers[key] = marker;

  marker.on("click", function () {
    highlightSidebar(key);
  });
});

// Highlight sidebar
function highlightSidebar(key) {
  document.querySelectorAll(".travel-item").forEach(el => el.classList.remove("active"));
  var item = document.querySelector(`.travel-item[data-loc="${key}"]`);
  if (item) item.classList.add("active");
}

// Sidebar click handler
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

