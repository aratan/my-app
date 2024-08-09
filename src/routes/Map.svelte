<script>
  import { onMount, onDestroy } from 'svelte';
  import { browser } from '$app/environment';

  let map;
  let intervalId;

  onMount(async () => {
    if (browser) {
      try {
        const L = await import('leaflet');
        await import('leaflet/dist/leaflet.css');

        if (navigator.geolocation) {
          const options = {
            enableHighAccuracy: true,
            timeout: 5000,
            maximumAge: 0
          };

          // Obtener la ubicación por primera vez
          navigator.geolocation.getCurrentPosition(
            position => initializeMap(L, position),
            showError,
            options
          );

          // Actualizar la ubicación cada 20 segundos
          intervalId = setInterval(() => {
            navigator.geolocation.getCurrentPosition(
              position => updateMapPosition(L, position),
              showError,
              options
            );
          }, 20000); // 20 segundos
        } else {
          alert('Geolocation is not supported by this browser.');
        }
      } catch (error) {
        console.error('Error loading Leaflet or initializing map:', error);
      }
    }
  });

  onDestroy(() => {
    if (intervalId) clearInterval(intervalId); // Limpiar el intervalo cuando se destruya el componente
  });

  function initializeMap(L, position) {
    const { latitude, longitude } = position.coords;

    map = L.map('map').setView([latitude, longitude], 18);
    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
      attribution: '© <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
    }).addTo(map);

    L.marker([latitude, longitude]).addTo(map)
      .bindPopup('You are here')
      .openPopup();
  }

  function updateMapPosition(L, position) {
    const { latitude, longitude } = position.coords;

    // Actualiza la vista del mapa y el marcador a la nueva posición
    map.setView([latitude, longitude], 18);
    const marker = L.marker([latitude, longitude]).addTo(map);
    marker.bindPopup('You are here').openPopup();
  }

  function showError(error) {
    let message = '';

    switch (error.code) {
      case error.PERMISSION_DENIED:
        message = 'User denied the request for Geolocation.';
        break;
      case error.POSITION_UNAVAILABLE:
        message = 'Location information is unavailable.';
        break;
      case error.TIMEOUT:
        message = 'The request to get user location timed out.';
        break;
      default:
        message = 'An unknown error occurred.';
        break;
    }

    alert(message);
  }
</script>

<style>
  #map {
    height: 400px;
    width: 100%;
  }
</style>

<div id="map"></div>
