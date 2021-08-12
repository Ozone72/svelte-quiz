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
    isAnswered = true;
    isCorrect = correct;
  }
</script>

<div>
  <h3>{@html question.question}</h3>

  {#if isAnswered}
    <h5>
      {#if isCorrect}
        You got it right!
      {:else}
        WRONG!
      {/if}
    </h5>
  {/if}

  {#each allAnswers as answer}
    <button disabled={isAnswered} on:click={() => checkQuestion(answer.correct)}
      >{@html answer.answer}
    </button>
  {/each}
</div>
<div>
  {#if isAnswered}
    <button on:click={nextQuestion}>Next Question</button>
  {/if}
</div>
