<script>
  import { fly } from "svelte/transition";
  // import { onMount, beforeUpdate, afterUpdate, onDestroy } from "svelte";
  import Question from "./Question.svelte";
  import Modal from "./Modal.svelte";
  import { score } from "./store";
  let activeQuestion = 0;
  let quiz = getQuiz();
  let isModalOpen = false;

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
    isModalOpen = false;
    score.set(0);
    activeQuestion = 0;
    quiz = getQuiz();
  }

  // this is a svelte a reactive statement
  $: if ($score > 7) {
    isModalOpen = true;
  }

  // reactive declaration
  $: questionNumber = activeQuestion + 1;
</script>

<div>
  <!-- could use the once event directive to only allow a new quiz if the current question > 10 -->
  <button on:click={resetQuiz}>Start New Quiz</button>

  <h3>My Score: {$score}</h3>
  <h4>Question #{questionNumber}</h4>
  <div class="container">
    {#await quiz}
      Loading...
    {:then data}
      {#each data.results as question, index}
        {#if index === activeQuestion}
          <div in:fly={{ x: 100 }} out:fly={{ x: -200 }} class="fade-wrapper">
            <Question {nextQuestion} {question} />
          </div>
        {/if}
      {/each}
    {/await}
  </div>
</div>

{#if isModalOpen}
  <!-- Here is where the custom component event 'close' is triggered -->
  <Modal on:close={resetQuiz}>
    <h2>You won!</h2>
    <p>You're a true cinephile</p>
    <button on:click={resetQuiz}>Start Over</button>
  </Modal>
{/if}

<style>
  .fade-wrapper {
    position: absolute;
  }

  .container {
    min-height: 500px;
  }
</style>
