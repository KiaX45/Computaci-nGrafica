<script>
	export let name;
</script>

<main>
	<h1>Hello {name}!</h1>
	<p>Visit the <a href="https://svelte.dev/tutorial">Svelte tutorial</a> to learn how to build Svelte apps.</p>



	<div class="container">
		<div class="row">
			<div class="col">
				<button type="button" class="btn btn-primary">Primary</button>
			</div>
			<div class="col">
				<button type="button" class="btn btn-secondary">Secondary</button>
			</div>
			<div class="col">
				<button type="button" class="btn btn-success">Success</button>
			</div>
			<div class="col">
				<button type="button" class="btn btn-danger">Danger</button>
			</div>
			<div class="col">
				<button type="button" class="btn btn-warning">Warning</button>
			</div>
			<div class="col">
				<button type="button" class="btn btn-info">Info</button>
			</div>
			<div class="col">
				<button type="button" class="btn btn-light">Light</button>
			</div>
			<div class="col">
				<button type="button" class="btn btn-dark">Dark</button>
			</div>
			<div class="col">
				<button type="button" class="btn btn-link">Link</button>
			</div>
		</div>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>