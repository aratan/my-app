<script>
	import Tablon from '../Tablon.svelte';
  
	const anuncios = [
	  { titulo: 'Fontanero', descripcion: 'Económico', contacto: 'tlf 65465454' },
	  { titulo: 'Niñera', descripcion: 'Fines de semana 16:00 a 00:00', contacto: 'tlf 12345678' }
	];
  </script>
  
  <svelte:head>
	<title>Tablon</title>
	<meta name="description" content="Svelte demo app" />
  </svelte:head>
  
  <section>
	<h1>
	  <span class="welcome"></span>
	  Tablon
	</h1>
  
	<h2>
	  Anuncios de <strong>Parroquianos</strong>
	</h2>
  
	<Tablon {anuncios} />
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
  
	.welcome img {
	  position: absolute;
	  width: 100%;
	  height: 100%;
	  top: 0;
	  display: block;
	}
  </style>
  