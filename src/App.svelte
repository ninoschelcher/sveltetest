<script>
	import Todos from "./components/Todos.svelte";
</script>

<main>
	<div>
		<h1 class="app-title">To-Do List</h1>
		<p id="undertitle">A small collection with to-do's</p>
		<Todos />
	</div>
</main>

<style>
	main {
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		font-family: "Inter";
		color: #070a39;
		font-weight: bold;
		font-size: 4em;
		margin: 0.2em 0;
	}

	p#undertitle {
		color: rgba(7, 10, 57, 0.6);
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>
