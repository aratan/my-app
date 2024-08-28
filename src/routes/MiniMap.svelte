<script>
  import { onMount } from 'svelte';
  let L;
  let map;

  export let lat = 0;
  export let lng = 0;
  

  onMount(async () => {
    if (typeof window !== 'undefined') { // Verifica si estás en el cliente
      // Carga dinámica del módulo Leaflet
      const leafletModule = await import('leaflet');
      L = leafletModule.default;
      import('leaflet/dist/leaflet.css'); // Importa el CSS de Leaflet

      try {
        map = L.map('map').setView([lat,lng], 18);

        L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
          attribution: '© <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
        }).addTo(map);
      } catch (error) {
        console.error("Error al inicializar el mapa:", error);
      }
    }
  });
</script>

<style>
  #map {
    height: 200px;
    width: 100%;
  }
</style>

<div id="map"></div>