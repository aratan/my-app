<script>
	import { onMount } from "svelte";
	import axios from "axios";
  
	let anuncios = [];
  
	onMount(async () => {
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
  
		// Verifica si los datos son un array y contienen elementos
		if (Array.isArray(data) && data.length > 0) {
		  anuncios = data;
		  console.log("Anuncios:", anuncios);
		} else {
		  console.error("Datos inesperados:", data);
		}
	  } catch (error) {
		console.error("Error en la solicitud:", error);
	  }
	});
  </script>
  
  <svelte:head>
	<title>Tablón</title>
	<meta name="description" content="Svelte demo app" />
  </svelte:head>
  
  <section class="container mt-5">
	<h1 class="text-center mb-4">Parroquia</h1>
  
	<div class="row">
	  {#each anuncios as anuncio}
		<!-- Filtrar tarjetas cuyo título no sea "Notificación" -->
		{#if anuncio.content?.title.toLowerCase() === "notificacion"}
		  <div class="col-md-4 mb-4">
			<div class="card">
			  <div class="card-header">
				<strong>{anuncio.content?.title || "Sin título"}</strong>
			  </div>
			  <div class="card-body">
				<!-- Mostrar icono si el 'photo' coincide con 'your_photo_url' -->
				{#if anuncio.from?.photo !== "your_photo_url"}
				  <img src="https://images.vexels.com/media/users/3/179548/isolated/preview/dccacfabeb7f8afe6fb3da7da10e6a21-templo-catedral-iglesia-detallada-silueta-by-vexels.png" alt="" width="30" height="30" />
				{/if}
  
				<!-- Mostrar el nombre de usuario si es diferente de 'your_username' -->
				{#if anuncio.from?.username !== "your_username"}
				  <p style="color: black;">{anuncio.from.username}</p>
				{/if}
  
				<!-- Mostrar subtítulo si existe -->
				{#if anuncio.content?.subtitle}
				  <p style="color: green;">{anuncio.content.subtitle}</p>
				{/if}
  
				<!-- Mostrar mensaje si existe -->
				{#if anuncio.content?.message}
				  <p style="color: blue;">{anuncio.content.message}</p>
				{/if}
			  </div>
			</div>
		  </div>
		{/if}
	  {/each}
	</div>
  </section>
  