<script>
    import { onMount } from 'svelte';
    import axios from 'axios';
  
    let anuncios = [];
    let searchTerm = '';
    let matchingAnuncio = null; // Almacenar la coincidencia específica
  
    // Función para cargar los anuncios desde la API
    async function loadAnuncios() {
      try {
        const response = await axios.post(
          "http://localhost:8080/api/recibe",
          {},
          {
            headers: {
              "Content-Type": "application/json",
            },
          }
        );
  
        const data = response.data;
        console.log("Datos recibidos de la API:", data);
  
        // Guardar los anuncios que tengan likes >= 1
        if (Array.isArray(data) && data.length > 0) {
          anuncios = data.filter(anuncio => anuncio.content.likes >= 1);
          console.log("Anuncios filtrados:", anuncios);
        } else {
          console.error("Datos inesperados:", data);
        }
      } catch (error) {
        console.error("Error en la solicitud:", error);
      }
    }
  
    // Ejecutar la carga de anuncios cuando el componente se monta
    onMount(() => {
      loadAnuncios();
    });
  
    // Función para filtrar anuncios por número de likes y mostrar coincidencias
    function filterAnuncios() {
      console.log("Término de búsqueda:", searchTerm);
      
      // Verificar si el término de búsqueda es un número
      if (!isNaN(searchTerm) && searchTerm !== '') {
        const searchLikes = Number(searchTerm);
        console.log("Filtrando por likes:", searchLikes);
        
        // Filtrar anuncios que tengan el número exacto de likes
        matchingAnuncio = anuncios.find(anuncio => anuncio.content.likes == 10);
        
        if (matchingAnuncio) {
          console.log("Coincidencia encontrada:", matchingAnuncio);
        } else {
          console.log("No se encontró ninguna coincidencia.");
        }
        
        return matchingAnuncio ? [matchingAnuncio] : [];
      }
  
      return [];
    }
  </script>
  
  <div class="container mt-4">
    <input
      type="text"
      class="form-control mb-3"
      placeholder="Buscar anuncios por número de likes..."
      bind:value={searchTerm}
    />
    
    <div class="row">
      {#each filterAnuncios() as anuncio}
        <div class="col-md-4">
          <div class="card mb-4 shadow-sm">
            <div class="card-body">
              <h5 class="card-title">{anuncio.content.title}</h5>
              <h6 class="card-subtitle mb-2 text-muted">{anuncio.content.subtitle}</h6>
              <p class="card-text">{anuncio.content.message}</p>
              <ul class="list-group list-group-flush">
                <li class="list-group-item"><strong>Likes:</strong> {anuncio.content.likes}</li>
                <li class="list-group-item"><strong>Subscribed:</strong> {anuncio.content.subscribed ? 'Yes' : 'No'}</li>
                <li class="list-group-item"><strong>Timestamp:</strong> {anuncio.timestamp}</li>
              </ul>
            </div>
          </div>
        </div>
      {/each}
    </div>
  </div>
  
  