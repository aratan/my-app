<script>
	import { page } from '$app/stores';

	
</script>

<footer>
	<div class="corner">
	pie de pagina
	</div>

	

	<div class="corner">
		<a href="https://github.com/sveltejs/kit">
			
		</a>
	</div>
</footer>

<style>
	footer {
		display: flex;
		justify-content: center; /* Centra el contenido horizontalmente */
	}

	.corner {
		width: 3em;
		height: 3em;
	}

	.corner a {
		display: flex;
		align-items: center;
		justify-content: center;
		width: 100%;
		height: 100%;
	}
</style>
