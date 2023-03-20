<script>
  export let trueModal = false
  export let falseModal = false
  export let falseNoPoints = false


  let closeModal = () => {
    trueModal = false
    falseModal = false
    falseNoPoints = false
  }
</script>


{#if trueModal}
  <div class="backdrop">
    <div class="trueModal">
      <h2>That is correct!</h2>
      <button on:click={closeModal}>Dismiss</button>
    </div>
  </div>
{/if}

{#if falseModal}
  <div class="backdrop">
    <div class="falseModal">
      <h2>Oh no... Thats wrong</h2>
      <button on:click={closeModal}>Dismiss</button>
    </div>
  </div>
{/if}

{#if falseNoPoints}
  <div class="backdrop">
    <div class="falseModal">
      <h2>Wrong answer... you also have no points</h2>
      <button on:click={closeModal}>Dismiss</button>
    </div>
  </div>
{/if}


<style>

</style>