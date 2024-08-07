<script>
    export let anuncios = [];
  </script>
  
  <style>
    .tablon {
      border: 1px solid #ccc;
      padding: 1rem;
      border-radius: 8px;
      width: 300px;
      margin: 1rem auto;
    }
  
    .anuncio {
      border-bottom: 1px solid #eee;
      padding: 0.5rem 0;
    }
  
    .anuncio:last-child {
      border-bottom: none;
    }
  </style>
  
  <div class="tablon">
    {#each anuncios as anuncio}
      <div class="anuncio">
        <strong>{anuncio.titulo}</strong><br />
        {anuncio.descripcion}<br />
        <em>{anuncio.contacto}</em>
      </div>
    {/each}
  </div>
  