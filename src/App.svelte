<script>/* https://opentdb.com/api.php?amount=5&difficulty=easy&type=multiple */
  import { onMount } from 'svelte'
  let promise
  
  onMount(async () => {
    const response = await fetch("https://opentdb.com/api.php?amount=1&type=boolean")
    promise = response.json()
  })

  let score = 0
  
</script>

<h1>Can you answer the question's ?</h1>

<h2 class="score">{score}</h2>


{#await promise}
  <div>Waiting...</div>

{:then results}
  {#if results !== undefined}
    {#each results.results as question}
      <p>{question.question}</p>
      <!-- <button on:click={trueAnswer}>True</button>
      <button on:click={falseAnswer}>False</button> -->
    {/each}
  <!-- <pre>{JSON.stringify(results, null, 2)}</pre> -->
  {/if}

{:catch error}
  <div>{error.message}</div>
{/await}



<style>
  h1 {text-align: center;}
  h2.score {text-align: center;}
</style>
