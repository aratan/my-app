<script>
    import { createEventDispatcher } from "svelte";
    import axios from "axios";
  
    let name = "";
    let mensaje = "";
    let geo = "";
    let horario = "";
    let iglesia = "";
    let dia = "";
  
    const dispatch = createEventDispatcher();
  
    async function createTablon() {
      try {
        const response = await axios.post(`http://localhost:8080/api/createTablon`, null, {
          params: {
            name,
            mensaje,
            geo,
            horario,
            iglesia,
            dia
          }
        });
  
        dispatch("tablonCreado", {
          content: {
            title: name,
            message: mensaje,
            geo,
            horario,
            iglesia,
            dia,
            likes: 0
          }
        });
  
        name = "";
        mensaje = "";
        geo = "";
        horario = "";
        iglesia = "";
        dia = "";
  
      } catch (error) {
        console.error("Error creating tablón:", error);
      }
    }
  </script>
  
  <div class="d-flex justify-content-center">
    <div class="card mb-4" style="max-width: 500px; width: 100%;">
      <div class="card-body">
        <h5 class="card-title text-center">Formulario de Creación de Tablón</h5>
        <form on:submit|preventDefault={createTablon}>
          <div class="form-group">
            <label for="name">Nombre del Tablón</label>
            <input type="text" id="name" class="form-control" bind:value={name} required />
          </div>
          <div class="form-group">
            <label for="mensaje">Mensaje</label>
            <textarea id="mensaje" class="form-control" bind:value={mensaje} required></textarea>
          </div>
          <div class="form-group">
            <label for="geo">Coordenadas Geográficas (ej: 40.123,-3.456)</label>
            <input type="text" id="geo" class="form-control" bind:value={geo} required />
          </div>
          <div class="form-group">
            <label for="horario">Horario</label>
            <input type="text" id="horario" class="form-control" bind:value={horario} required />
          </div>
          <div class="form-group">
            <label for="iglesia">Iglesia</label>
            <input type="text" id="iglesia" class="form-control" bind:value={iglesia} required />
          </div>
          <div class="form-group">
            <label for="dia">Día</label>
            <input type="text" id="dia" class="form-control" bind:value={dia} required />
          </div>
          <button type="submit" class="btn btn-success mt-2 w-100">Crear Tablón</button>
        </form>
      </div>
    </div>
  </div>
  