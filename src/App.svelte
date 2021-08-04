<script>
	import Quiz from "./Quiz.svelte";
	export let name = "Suckah";
</script>

<main>
	<h1>Hello {name}!</h1>
	<Quiz quizName="New Quiz" />
</main>
