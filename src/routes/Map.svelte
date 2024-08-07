<script>
    import { onMount } from 'svelte';
    import * as L from 'leaflet';
    import 'leaflet/dist/leaflet.css';
  
    let map;
  
    onMount(() => {
      map = L.map('map').setView([40.29753, -3.83810], 18);
      L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
        attribution: '© <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
      }).addTo(map);
    });
  </script>
  
  <style>
    #map {
      height: 400px;
      width: 100%;
    }
  </style>
  
  <div id="map"></div>
  