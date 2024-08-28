<script>
  import { onMount } from "svelte";
  import axios from "axios";
  import FormularioTablon from "../FormularioTablon.svelte";
  
  let tablones = [];
  let loading = true;
  let error = null;
  
  async function fetchTablones() {
    try {
      const response = await fetch("http://localhost:8080/api/readTablon");
  
      if (!response.ok) {
        throw new Error("Error al obtener los datos de los tablones");
      }
  
      tablones = await response.json();
    } catch (err) {
      error = err.message;
    } finally {
      loading = false;
    }
  }
  
  async function deleteTablon(tablonIndex) {
    try {
      await axios.delete(`http://localhost:8080/api/deleteTablonByIndex?index=${tablonIndex}`);
      tablones = tablones.filter((_, i) => i !== tablonIndex); // Actualiza la lista de tablones
    } catch (error) {
      console.error("Error deleting tablon:", error);
    }
  }

  async function addLike(tablonId) {
    try {
      const response = await axios.post(`http://localhost:8080/api/addLikeToTablon?tablon_id=${tablonId}`);
      if (response.status === 200) {
        const updatedTablon = tablones.find(tablon => tablon.id === tablonId);
        updatedTablon.likes += 1;
        tablones = [...tablones]; // Reasignar para forzar reactividad
      }
    } catch (error) {
      console.error("Error adding like:", error);
    }
  }
  
  onMount(fetchTablones);
</script>
  
<div class="container mt-4">
  <h1>Tablones</h1>
  <p>Bienvenido a la página de los tablones.</p>
  <FormularioTablon />
  
  {#if loading}
    <p>Cargando...</p>
  {:else if error}
    <p>Error: {error}</p>
  {:else}
    <div class="row">
      {#each tablones as tablon, tablonIndex}
        <div class="col-md-4 mb-4">
          <div class="card">
            <div class="card-header">
              <h5 class="card-title">{tablon.name}</h5>
            </div>
            <div class="card-body">
              {#each tablon.messages as message, messageIndex}
                <div class="message">
                  <h6 class="card-subtitle mb-2 text-muted">{message.content.subtitle}</h6>
                  <p class="card-text">{message.content.message}</p>
                  <small class="text-muted">Enviado por {message.from.username} el {new Date(message.timestamp).toLocaleString()}</small>
                  
                  <!-- Botón de Like -->
                  <button class="btn btn-primary" on:click={() => addLike(tablon.id)}>Like</button>
                </div>
                <hr />
              {/each}
              <p><strong>Geolocalización:</strong> {tablon.geo || "No disponible"}</p>
            </div>
            <div class="card-footer">
              <!-- Botón de Eliminar Tablón -->
              <button class="btn btn-danger" on:click={() => deleteTablon(tablonIndex)}>Eliminar Tablón</button>
              <p>Likes: {tablon.likes}</p>
            </div>
          </div>
        </div>
      {/each}
    </div>
  {/if}
</div>

<style>
  .container {
    max-height: 80vh; /* Permitir scroll vertical */
    overflow-y: auto;
  }

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

  .btn-primary {
    background-color: #007bff;
    border-color: #007bff;
  }
  /* Para navegadores WebKit (Chrome, Safari, etc.) */
.container::-webkit-scrollbar {
  width: 0;  /* Ancho del scrollbar */
  height: 0; /* Alto del scrollbar (horizontal) */
}

/* Para navegadores Firefox */
.container {
  scrollbar-width: none; /* Ocultar scrollbar en Firefox */
}

/* Para todos los navegadores, ocultar scrollbar */
.container {
  -ms-overflow-style: none; /* Ocultar scrollbar en IE y Edge */
  overflow: -moz-scrollbars-none; /* Ocultar scrollbar en versiones antiguas de Firefox */
}

</style>
