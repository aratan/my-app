<script>
	import Map from './Map.svelte';
	import Buscador from './Buscador.svelte'; // Importar el componente Buscador
  </script>
  
  <svelte:head>
	<title>Home</title>
	<meta name="description" content="Svelte demo app" />
  </svelte:head>
  
  <section>
	<h1>
	  <span class="welcome"></span>
	  cercanas<br />5 km
	</h1>
  
	<Map />
  
	<h2>
	  Parroquias <strong>Catolicas</strong>
	</h2>
	<!-- Usar el componente Buscador -->
	<Buscador />
  </section>
  
  <style>
	section {
	  display: flex;
	  flex-direction: column;
	  justify-content: center;
	  align-items: center;
	  flex: 0.6;
	}
  
	h1 {
	  width: 100%;
	}
  
	.welcome {
	  display: block;
	  position: relative;
	  width: 100%;
	  height: 0;
	  padding: 0 0 calc(100% * 495 / 2048) 0;
	}
  </style>
  