<script>
  import { onMount } from "svelte";
  import axios from "axios";

  let anuncios = [];

  // Cargar los anuncios desde la API cuando se monte el componente
  onMount(async () => {
    try {
      const response = await axios.post("http://localhost:8080/api/recibe");
      anuncios = response.data;
    } catch (error) {
      console.error("Error fetching data:", error);
    }
  });

  // Función para eliminar un anuncio por su índice
  async function deleteAnuncio(index) {
    try {
      await axios.delete(`http://localhost:8080/api/deleteMessageByIndex?index=${index}`);
      // Remover el anuncio de la lista local después de eliminarlo del servidor
      anuncios = anuncios.filter((_, i) => i !== index);
    } catch (error) {
      console.error("Error deleting anuncio:", error);
    }
  }
</script>

<div class="container mt-4">
  <h1 class="text-center mb-4">Lista de Anuncios</h1>
  <div class="row">
    {#each anuncios as anuncio, index}
      <div class="col-md-4 mb-4">
        <div class="card">
          <div class="card-body">
            <h5 class="card-title">{anuncio.content.title || "Sin título"}</h5>
            <p class="card-text">{anuncio.content.message}</p>
            <button class="btn btn-danger" on:click={() => deleteAnuncio(index)}>Eliminar</button>
          </div>
        </div>
      </div>
    {/each}
  </div>
</div>

<style>
  .card {
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    transition: 0.3s;
  }

  .card:hover {
    box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
  }

  .card-title {
    font-weight: bold;
  }
</style>
