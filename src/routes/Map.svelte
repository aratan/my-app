<script>
  import { onMount } from 'svelte';
  import { browser } from '$app/environment';

  let map;

  onMount(async () => {
      if (browser) {
          const L = await import('leaflet');
          await import('leaflet/dist/leaflet.css');

          if (navigator.geolocation) {
              const options = {
                  enableHighAccuracy: true,
                  timeout: 5000,
                  maximumAge: 0
              };

              navigator.geolocation.getCurrentPosition(position => {
                  const { latitude, longitude } = position.coords;

                  map = L.map('map').setView([latitude, longitude], 18);
                  console.log(latitude, longitude )
                  L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
                      attribution: '© <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
                  }).addTo(map);

                  L.marker([latitude, longitude]).addTo(map)
                      .bindPopup('You are here')
                      .openPopup();
              }, showError, options);
          } else {
              alert("Geolocation is not supported by this browser.");
          }
      }
  });

  function showError(error) {
      switch (error.code) {
          case error.PERMISSION_DENIED:
              alert("User denied the request for Geolocation.");
              break;
          case error.POSITION_UNAVAILABLE:
              alert("Location information is unavailable.");
              break;
          case error.TIMEOUT:
              alert("The request to get user location timed out.");
              break;
          case error.UNKNOWN_ERROR:
              alert("An unknown error occurred.");
              break;
      }
  }
</script>

<style>
  #map {
      height: 400px;
      width: 100%;
  }
</style>

<div id="map"></div>
