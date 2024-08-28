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

  // Función para añadir un like a un anuncio por su índice
  async function addLike(index) {
    try {
      await axios.post(`http://localhost:8080/api/addLikeToMessage?index=${index}`);
      // Actualizar el número de likes localmente después de añadir el like
      anuncios = anuncios.map((anuncio, i) =>
        i === index ? { ...anuncio, content: { ...anuncio.content, likes: anuncio.content.likes + 1 } } : anuncio
      );
    } catch (error) {
      console.error("Error adding like:", error);
    }
  }
</script>

<div class="container mt-4">
  <h1 class="text-center mb-4">Lista de Anuncios</h1>
  <div class="row">
    {#each anuncios as anuncio, index}
      <div class="col-md-4 mb-4">
        <div class="card">
          <img src={anuncio.from.photo} class="card-img-top" alt="{anuncio.from.username}">
          <div class="card-body">
            <h5 class="card-title">{anuncio.content.title || "Sin título"}</h5>
            <h6 class="card-subtitle mb-2 text-muted">{anuncio.content.subtitle}</h6>
            <p class="card-text">{anuncio.content.message}</p>
            <p class="card-text"><small class="text-muted">Enviado por: {anuncio.from.username}</small></p>
            <p class="card-text"><small class="text-muted">Destino: {anuncio.to}</small></p>
            <p class="card-text"><small class="text-muted">Fecha: {new Date(anuncio.timestamp).toLocaleString()}</small></p>
            <button class="btn btn-primary me-2" on:click={() => addLike(index)}>Like</button>
            <button class="btn btn-danger" on:click={() => deleteAnuncio(index)}>Eliminar</button>
            <p>Likes: {anuncio.content.likes > 0 ? anuncio.content.likes : 0}</p>
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

  .card-subtitle {
    font-weight: normal;
    font-size: 14px;
  }

  .card-text small {
    display: block;
    margin-top: 5px;
  }

  .card-img-top {
    height: 180px;
    object-fit: cover;
  }
</style>
