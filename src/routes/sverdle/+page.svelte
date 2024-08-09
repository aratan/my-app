<script>
	import { onMount } from "svelte";
	import axios from 'axios';
  
	let anuncios = [];
	const token = "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJhdXRob3JpemVkIjp0cnVlLCJleHAiOjE3MjMyMzk3NjQsInVzZXIiOiJ0dV91c3VhcmlvIn0.qnBq6isuk8jm8sp_wb-htX0H7Fvyz-aXR2c0U5yKs9Q";
  
	onMount(async () => {
		try {
			const response = await axios.post('http://localhost:8080/api/recibe', {}, {
				headers: {
					'Content-Type': 'application/json',
					'Authorization': `Bearer ${token}`
				}
			});
	  
			const data = response.data;
			console.log("Datos recibidos de la API:", data);
	  
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
	<title>Tablon</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>
  
<section class="container mt-5">
	<h1 class="text-center mb-4">Anuncios de Parroquianos</h1>
	
	<div class="row">
		{#each anuncios as anuncio}
			<div class="col-md-4 mb-4">
				<div class="card">
					<div class="card-header">
						<strong>{anuncio.content?.title || 'Sin título'}</strong>
					</div>
					<div class="card-body">
						{#if anuncio.from?.photo}
							<p >{anuncio.from.photo} </p>
						{/if}
						
						{#if anuncio.from?.username}
							<p style="color: green;">{anuncio.from.username}</p>
						{/if}
						
						{#if anuncio.content?.subtitle}
							<p style="color: green;">{anuncio.content.subtitle}</p>
						{/if}
						
						{#if anuncio.content?.message}
							<p style="color: blue;">{anuncio.content.message}</p>
						{/if}
					</div>
				</div>
			</div>
		{/each}
	</div>
</section>
  
