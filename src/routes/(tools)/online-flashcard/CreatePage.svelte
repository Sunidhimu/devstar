<script>
  import { onMount } from 'svelte';
  import { goto } from '@sveltejs/kit';

  let studySets = [];
  let inputs = [{ term: "", definition: "" }];
  let title = "";

  function addInput() {
    inputs = [...inputs, { term: "", definition: "" }];
  }

  function removeInput(index) {
    inputs = inputs.filter((_, i) => i !== index);
  }

  function updateInput(index, field, value) {
    inputs[index][field] = value;
  }

  function saveSet() {
    let newSet = [{ title }];
    for (let input of inputs) {
      newSet.push({ [input.term]: input.definition });
    }
    let numSets = localStorage.getItem('set#');
    numSets = numSets ? Number(numSets) + 1 : 1;
    localStorage.setItem(numSets, JSON.stringify(newSet));
    localStorage.setItem('set#', numSets);
    goto('/home');
  }

  onMount(() => {
    let numOfSets = localStorage.getItem('set#');
    if (numOfSets > 0) {
      for (let i = 1; i <= numOfSets; i++) {
        let setData = JSON.parse(localStorage.getItem(i));
        let setTitle = setData[0]['title'];
        studySets.push({ id: i, title: setTitle });
      }
    }
  });
</script>

<style>
  /* Add your CSS here */
</style>

<div>
  <header>
    <div class="container">
      <div class="main">Flashcard</div>
      <div class="left">
        <a href="/+page.svelte" class="icon-link">
          <div>
            <svg xmlns="http://www.w3.org/2000/svg" width="40" height="40" fill="currentColor" class="bi bi-collection" viewBox="0 0 16 16">
              <path d="M2.5 3.5a.5.5 0 0 1 0-1h11a.5.5 0 0 1 0 1zm2-2a.5.5 0 0 1 0-1h7a.5.5 0 0 1 0 1zM0 13a1.5 1.5 0 0 0 1.5 1.5h13A1.5 1.5 0 0 0 16 13V6a1.5 1.5 0 0 0-1.5-1.5h-13A1.5 1.5 0 0 0 0 6zm1.5.5A.5.5 0 0 1 1 13V6a.5.5 0 0 1 .5-.5h13a.5.5 0 0 1 .5.5v7a.5.5 0 0 1-.5.5z" />
            </svg>
          </div>
        </a>
        <a href="/create" class="icon-link">
          <div>
            <svg xmlns="http://www.w3.org/2000/svg" width="40" height="40" fill="currentColor" class="bi bi-file-plus" viewBox="0 0 16 16">
              <path d="M8.5 6a.5.5 0 0 0-1 0v1.5H6a.5.5 0 0 0 0 1h1.5V10a.5.5 0 0 0 1 0V8.5H10a.5.5 0 0 0 0-1H8.5z" />
              <path d="M2 2a2 2 0 0 1 2-2h8a2 2 0 0 1 2 2v12a2 2 0 0 1-2 2H4a2 2 0 0 1-2-2zm10-1H4a1 1 0 0 0-1 1v12a1 1 0 0 0 1 1h8a1 1 0 0 0 1-1V2a1 1 0 0 0-1-1" />
            </svg>
          </div>
        </a>
        <div>
          <svg xmlns="http://www.w3.org/2000/svg" width="40" height="40" fill="currentColor" class="bi bi-person-square" viewBox="0 0 16 16">
            <path d="M11 6a3 3 0 1 1-6 0 3 3 0 0 1 6 0" />
            <path d="M2 0a2 2 0 0 0-2 2v12a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V2a2 2 0 0 0-2-2zm12 1a1 1 0 0 1 1 1v12a1 1 0 0 1-1 1v-1c0-1-1-4-6-4s-6 3-6 4v1a1 1 0 0 1-1-1V2a1 1 0 0 1 1-1z" />
          </svg>
        </div>
      </div>
    </div>
  </header>
</div>
<hr>
<div class="container">
  <h1>Create New Study Set</h1>
  <div class="input-group">
    <label for="title">Title</label>
    <input type="text" id="title" bind:value={title} />
  </div>
  <div class="input-group">
    <h2>Terms and Definitions</h2>
    {#each inputs as input, index}
      <div class="term-def-group">
        <input type="text" placeholder="Term" bind:value={input.term} on:input={(e) => updateInput(index, 'term', e.target.value)} />
        <input type="text" placeholder="Definition" bind:value={input.definition} on:input={(e) => updateInput(index, 'definition', e.target.value)} />
        <button on:click={() => removeInput(index)}>Remove</button>
      </div>
    {/each}
  </div>
  <button on:click={addInput}>Add Term/Definition</button>
  <button on:click={saveSet}>Save Study Set</button>
</div>
