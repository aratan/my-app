<script>
  import { onMount, onDestroy } from 'svelte';
  import { browser } from '$app/environment';
  let map;
  let intervalId;
  let routingControl;
  let userMarker;

  onMount(async () => {
    if (browser) {
      try {
        const L = await import('leaflet');
        await import('leaflet/dist/leaflet.css');
        const Routing = await import('leaflet-routing-machine');

        if (navigator.geolocation) {
          const options = {
            enableHighAccuracy: true,
            timeout: 5000,
            maximumAge: 0
          };

          // Obtener la ubicación por primera vez
          navigator.geolocation.getCurrentPosition(
            position => initializeMap(L, Routing, position),
            showError,
            options
          );

          // Actualizar la ubicación cada 20 segundos
          intervalId = setInterval(() => {
            navigator.geolocation.getCurrentPosition(
              position => updateMapPosition(L, Routing, position),
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

  function initializeMap(L, Routing, position) {
    const { latitude, longitude } = position.coords;
    const destination = [40.4168, -3.7038]; // Coordenadas de la iglesia (puedes cambiarlas)
    const greenMarkerPosition = [40.2976198335607, -3.8390999113944533]; // Coordenadas para el pin verde

    map = L.map('map').setView([latitude, longitude], 13);

    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
      attribution: '© <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
    }).addTo(map);

    // Añadir el pin en la ubicación actual
    userMarker = L.marker([latitude, longitude]).addTo(map)
      .bindPopup('You are here')
      .openPopup();

    // Añadir el control de ruta
    routingControl = Routing.control({
      waypoints: [
        L.latLng(latitude, longitude),
        L.latLng(destination[0], destination[1])
      ],
      routeWhileDragging: true
    }).addTo(map);

    // Crear un icono personalizado para el pin verde
    const greenIcon = L.icon({
      iconUrl: 'https://cdnjs.cloudflare.com/ajax/libs/leaflet/1.7.1/images/marker-icon-2x-green.png', // URL del icono verde
      iconSize: [25, 41], // Tamaño del icono
      iconAnchor: [12, 41], // Punto de anclaje del icono
      popupAnchor: [1, -34], // Punto de anclaje del popup
      shadowUrl: 'https://cdnjs.cloudflare.com/ajax/libs/leaflet/1.7.1/images/marker-shadow.png', // Sombra del icono
      shadowSize: [41, 41], // Tamaño de la sombra
      shadowAnchor: [12, 41] // Punto de anclaje de la sombra
    });

    // Añadir el pin verde en la posición especificada
    L.marker(greenMarkerPosition, { icon: greenIcon }).addTo(map)
      .bindPopup('Posición personalizada en verde')
      .openPopup();
  }

  function updateMapPosition(L, Routing, position) {
    const { latitude, longitude } = position.coords;
    const destination = [40.4168, -3.7038]; // Coordenadas de la iglesia (puedes cambiarlas)
    
    // Actualiza la vista del mapa a la nueva posición
    map.setView([latitude, longitude], 13);

    // Mover el pin a la nueva posición
    if (userMarker) {
      userMarker.setLatLng([latitude, longitude]).update();
      userMarker.bindPopup('You are here').openPopup();
    }

    // Actualiza la ruta con la nueva posición
    routingControl.setWaypoints([
      L.latLng(latitude, longitude),
      L.latLng(destination[0], destination[1])
    ]);
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
