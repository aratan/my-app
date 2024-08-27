<script>
    import { onMount } from "svelte";
  
    // Variables reactivas para almacenar la lista de tablones
    let tablones = [];
    let loading = true;
    let error = null;
  
    // Función que hace la solicitud a la API para obtener todos los tablones
    async function fetchTablones() {
      try {
        const response = await fetch("http://localhost:8080/api/readTablon");
  
        if (!response.ok) {
          throw new Error("Error al obtener los datos de los tablones");
        }
  
        // Convierte la respuesta en JSON
        const data = await response.json();
        tablones = data;
      } catch (err) {
        error = err.message;
      } finally {
        loading = false;
      }
    }
  
    // Ejecuta la función cuando se carga el componente
    onMount(fetchTablones);
  </script>
  
  <div class="container mt-4">
    <h1>Tablones</h1>
    <p>Bienvenido a la página de los tablones.</p>
  
    <!-- Muestra un mensaje de carga mientras se obtienen los datos -->
    {#if loading}
      <p>Cargando...</p>
    {:else if error}
      <p>Error: {error}</p>
    {:else}
      <!-- Muestra la lista de todos los tablones cuando se han cargado -->
      <div class="row">
        {#each tablones as tablon}
          <div class="col-md-4 mb-4">
            <div class="card">
              <div class="card-header">
                <h5 class="card-title">{tablon.name}</h5>
              </div>
              <div class="card-body">
                {#each tablon.messages as message}
                  <div class="message">
                    <h6 class="card-subtitle mb-2 text-muted">{message.content.subtitle}</h6>
                    <p class="card-text">{message.content.message}</p>
                    <small class="text-muted">Enviado por {message.from.username} el {new Date(message.timestamp).toLocaleString()}</small>
                  </div>
                  <hr />
                {/each}
                <!-- Muestra la geolocalización -->
                <p><strong>Geolocalización:</strong> {tablon.geo || "No disponible"}</p>
              </div>
              <div class="card-footer">
                <p>Likes: {tablon.messages[0].content.likes}</p>
              </div>
            </div>
          </div>
        {/each}
      </div>
    {/if}
  </div>
  
  <style>
    /* Estilos específicos para tu página */
    .card {
      box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
      transition: 0.3s;
    }
  
    .card:hover {
      box-shadow: 0 8px 16px 0 rgba(0, 0, 0, 0.2);
    }
  
    .card-header {
      background-color: #007bff;
      color: white;
    }
  </style>
  