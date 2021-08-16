<script>
  import { fly } from "svelte/transition";
  import { onMount, beforeUpdate, afterUpdate, onDestroy } from "svelte";
  import Question from "./Question.svelte";
  let activeQuestion = 0;
  let score = 0;
  let quiz = getQuiz();

  onMount(() => {
    console.log("I mounted. Would mount again.");
  });

  beforeUpdate(() => {
    console.log("before update, I will destroy you.");
  });

  afterUpdate(() => {
    console.log("after update, I will attain glory.");
  });

  onDestroy(() => {
    console.log("Well, let's go again!");
  });

  async function getQuiz() {
    const res = await fetch(
      "https://opentdb.com/api.php?amount=10&category=11&difficulty=easy&type=multiple"
    );
    const quiz = await res.json();
    return quiz;
  }

  function nextQuestion() {
    activeQuestion = activeQuestion + 1;
  }

  function resetQuiz() {
    score = 0;
    activeQuestion = 0;
    quiz = getQuiz();
  }

  function addToScore() {
    score = score + 1;
  }

  // this is a svelte a reactive statement
  $: if (score > 7) {
    alert("Not too bad Ebert!");
    resetQuiz();
  }

  // reactive declaration
  $: questionNumber = activeQuestion + 1;
</script>

<!--  -->
<div>
  <button on:click={resetQuiz}>Start New Quiz</button>

  <h3>My Score: {score}</h3>
  <h4>Question #{questionNumber}</h4>

  {#await quiz}
    Loading...
  {:then data}
    {#each data.results as question, index}
      {#if index === activeQuestion}
        <div in:fly={{ x: 100 }} out:fly={{ x: -200 }} class="fade-wrapper">
          <Question {addToScore} {nextQuestion} {question} />
        </div>
      {/if}
    {/each}
  {/await}
</div>

<style>
  .fade-wrapper {
    position: absolute;
  }
</style>
