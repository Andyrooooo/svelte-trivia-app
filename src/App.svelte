<script lang="ts">/* https://opentdb.com/api.php?amount=5&difficulty=easy&type=multiple */
  /* import Modal from './modal.svelte' */
  let trueModal = false
  let falseModal = false
  let falseNoPoints = false
  let promise
  let score = 0
  let difficulty
  let category
  let toggle = false
  let anotherGame = true
  let hideTheQuestion = false
  let answerClicked = false

// ------------------------------ Allows us to fetch the data for our API, and closes the form ------------------
  export async function fetchData() {
    const answer = {
      difficulty,
      category
    }

    const response = await fetch(`https://opentdb.com/api.php?amount=1&category=${answer.category}&difficulty=${answer.difficulty}&type=boolean`);
    if (response.ok) {
      promise = await response.json();
    } else {
      console.error('Failed to fetch API data:', response.statusText);
    }
    
    toggle = true
    hideTheQuestion = false
    answerClicked = false
}

// ------------------------------ True answer button that will display the reset button, and true and false buttons based on question ------------------
const trueAnswer = (correct_answer) => {
    let answer = "True"
    anotherGame = false
    answerClicked = true
    if (answer === correct_answer) {
      score += 1
      anotherGame = false
      trueModal = true
    }
    if (answer != correct_answer) {
        if (score < 1 ) {
          falseNoPoints = true
          return score
        } else {
          score -= 1
          falseModal = true
        }
    }
    /* console.log(correct_answer, incorrect_answers)
    console.log(score) */
  }

// ------------------------------ False answer button that will display the reset button, and true and false buttons based on question ------------------
  const falseAnswer = (correct_answer) => {
    let answer = "False"
    anotherGame = false
    answerClicked = true
    if (answer === correct_answer) {
      score += 1
      anotherGame = false
      trueModal = true
    }
    if (answer != correct_answer) {
        if (score < 1 ) {
          falseNoPoints = true
          return score
        } else {
          score -= 1
          falseModal = true
        }
    }
    /* console.log(correct_answer, incorrect_answers)
    console.log(score) */
    
  }

  // ------------------------------ New game function will open the form again and allow for a new question ------------------
  let newGame = () => {
    toggle = false
    anotherGame = true
    hideTheQuestion = true
  }

  let closeModal = () => {
    trueModal = false
    falseModal = false
    falseNoPoints = false
  }
  
</script>

<!-------------------------------- component will display our true, false alerts -------------------->
{#if trueModal}
  <div class="backdrop">
    <div class="trueModal">
      <h2>That is correct!</h2>
      <button class="closeButton" on:click={closeModal}>Dismiss</button>
    </div>
  </div>
{/if}

{#if falseModal}
  <div class="backdrop">
    <div class="falseModal">
      <h2>Oh no... Thats wrong</h2>
      <button class="closeButton" on:click={closeModal}>Dismiss</button>
    </div>
  </div>
{/if}

{#if falseNoPoints}
  <div class="backdrop">
    <div class="falseModal">
      <h2>Wrong answer... you also have no points</h2>
      <button class="closeButton" on:click={closeModal}>Dismiss</button>
    </div>
  </div>
{/if}


<div class="title">
<h1 class="title1">T</h1>
<h1 class="title2">R</h1>
<h1 class="title3">I</h1>
<h1 class="title4">V</h1>
<h1 class="title5">I</h1>
<h1 class="title6">A</h1>
</div>

<h2 class="score">{score}</h2>



<div class:hidden={toggle}>
  <div class="form">
    <div class="difficulty">
      <h3>Select Difficulty Level</h3>
      <select bind:value={difficulty}>
        <option value="hard">Hard</option>
        <option value="medium">Medium</option>
        <option value="easy">Easy</option>
      </select>
    </div>
    
    <div class="category">
      <h3>Select a category</h3>
      <select bind:value={category}>
        <option value="{9}">General Knowledge</option>
        <option value="{11}">Entertainment: Film</option>
        <option value="{12}">Entertainment: Music</option>
        <option value="{13}">Entertainment: Musicals & Theaters</option>
        <option value="{15}">Entertainment: Video Games</option>
        <option value="{16}">Entertainment: Board Games</option>
        <option value="{18}">Science: Computers</option>
        <option value="{21}">Sports</option>
        <option value="{22}">Geography</option>
        <option value="{23}">History</option>
        <option value="{24}">Celebrities</option>
        <option value="{25}">Art</option>
        <option value="{28}">Vehicles</option>
        <option value="{30}">Science: Gadgets</option>
      </select>
    </div>
    
    <div class="letsGo">
      <button on:click={fetchData}>Lets Go!</button>
    </div>
  </div>
</div>


{#await promise}
  <div>Waiting...</div>

{:then results}
  {#if results !== undefined}
    {#each results.results as question}
      <div class:hideQuestion={hideTheQuestion}>
        <p class="question">{question.question}</p>
      </div>
      <div class:hideButtons={answerClicked}>
          <button class="trueButton" on:click={() => trueAnswer(question.correct_answer)}>True</button>
          <button class="falseButton" on:click={() => falseAnswer(question.correct_answer)}>False</button>  
      </div>
      <!-- <pre>{JSON.stringify(results, null, 2)}</pre> -->
      <button class="replayButton" class:hide={anotherGame} on:click={newGame}>Wanna play another Game?</button>
    {/each}
  {/if}

{:catch error}
  <div>{error.message}</div>
{/await}