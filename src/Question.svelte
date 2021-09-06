<script>
  export let question;
  export let nextQuestion;
  export let addToScore;

  let answers = question.incorrect_answers.map((answer) => {
    return {
      answer,
      correct: false,
    };
  });

  let isCorrect;
  let isAnswered = false;
  // spread
  let allAnswers = [
    ...answers,
    {
      answer: question.correct_answer,
      correct: true,
    },
  ];

  // classic shuffle algorithm for arrays
  const shuffle = (answers) => {
    for (let i = answers.length - 1; i > 0; i--) {
      const randAnswer = Math.floor(Math.random() * i);
      const temp = answers[i];

      answers[i] = answers[randAnswer];
      answers[randAnswer] = temp;
    }
  };

  // randomizes elements in the answers array
  shuffle(allAnswers);

  function checkQuestion(correct) {
    if (!isAnswered) {
      isAnswered = true;
      isCorrect = correct;
      // adds to score variable if is correct
      if (correct) {
        addToScore();
      }
    }
  }
</script>

<h3>{@html question.question}</h3>

{#if isAnswered}
  <!-- <h5 class={isCorrect ? "correct" : "wrong"}> -->
  <!-- <h5 class:correct={isCorrect}> -->
  <h5 class:isCorrect class:isWrong={!isCorrect}>
    {#if isCorrect}
      You got it right!
    {:else}
      Wrong answer! I'll pop the popcorn...
    {/if}
  </h5>
{/if}

{#each allAnswers as answer}
  <button disabled={isAnswered} on:click={() => checkQuestion(answer.correct)}
    >{@html answer.answer}
  </button>
{/each}

{#if isAnswered}
  <div>
    <button on:click={nextQuestion}>Next Question</button>
  </div>
{/if}

<style>
  h5.isWrong {
    color: red;
  }

  h5.isCorrect {
    color: green;
  }
</style>
