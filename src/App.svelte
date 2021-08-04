<script>
	import Quiz from "./Quiz.svelte";
	export let name = "Suckah";
</script>

<main>
	<h1>Hello {name}!</h1>
	<h4>I'm an h4!!</h4>
	<Quiz />
</main>

<style>
	:global(h4) {
		color: green;
	}
</style>
